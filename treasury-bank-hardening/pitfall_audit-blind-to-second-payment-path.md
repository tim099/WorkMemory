---
id: pitfall_audit-blind-to-second-payment-path
topic: treasury-bank-hardening
title: 稽核與補款共用判準卻各建「已付」集合 —— 一盞每天亮的假警報，而兩支都沒有壞
type: pitfall
status: active
created_at: 2026-09-23
created_by: basecamp
links: [pitfall_self-declared-field-as-identity]
related_docs: []
---

**症狀**：`payroll-audit` 說 2026-09-22 **差 114**，`op=post_reward_backfill`（唯讀試算）說**該補 0**。
兩支都沒有壞，而且它們**共用同一個發放判準**（`Cmd_Tavern.IsPostRewardEligible`）。

**成因（讀數，⛔ 不是推論）**

| 端 | 「已付」集合從哪裡建 |
|---|---|
| `UCL_TavernPostRewardBackfill` | `kind=work_post` 的分錄 **＋** `<BankRoot>/payroll_settled.json` 的 `settled[].refs` |
| `SCP_PayrollAudit` | 只有 `kind=work_post` 的分錄 —— **全檔沒有提過那個檔名** |

⇒ 走**請款補發**（`payout_request`）的那些則不帶逐則 `ref`，所以它們另記在那份清單裡。
稽核讀不到 ⇒ 每天把它們報成缺口。

**逐則對帳**（tavern 房 09-22 共 279 則）：`work_post` 落帳 **161**／清單結清 **114**／
酒保 **4**（結構上不計酬）＝ 279 ⇒ **零缺口**。
`demo` 房那 1 筆是 `probe-0273`（刻意解析不到帳號的反向對照探針）。

**🩸 而最該記的是這一格的由來**

本檔的 `CompensationEntries` 註解**當時就寫對了**：
「稽核會每天對那一天亮燈 —— 一面永遠亮紅燈的儀表，人會在第三天學會忽略它。」
而作者（我）判斷「要逐則證明，而**證據不存在**」⇒ 選擇只把金額擺在旁邊。

**那句話寫下時為真** —— `payroll_settled.json` 是後來才長出來的。
而**沒有任何一層回來更新它** ⇒ 一個寫對了的定語，自己變成一盞假警報。

**修法（`SCP_Core 7ebf1d2`）**

- 併入 `settled[].refs`，但**分欄計數**（新欄位 `Settled`），⛔ 不併進 `Paid` ——
  兩者憑據強度不同（一個背後是帶 ref 的分錄，一個只有一份清單）。
- 判定順序：**帶 ref 的分錄優先** ⇒ 兩邊都有時不會各加一次。
- 綠燈要說得出自己怎麼綠的（`差 0` 在兩種情況下同形）。
- 🔴 守衛：**當天有請款撥款 ＋ 有差集 ＋ 清單不在** ⇒ 出聲。
  ⛔ 不是「檔不在就叫」（那會讓乾淨的樹每天多一行假警告），三個條件都是讀數。

**⚠ 讓守衛紅的那一步不可以用改名**

`UCL_TavernPostRewardBackfill` 在該檔不存在時是 `if (File.Exists(...))` ⇒ **靜默跳過**
⇒ 那 114 則會變回 `eligible` ⇒ **憑空增發**。
⇒ 改用淨室（只複製 09-22 起的 `Bank/ledger` ＋ 當天全房訊息，刻意不複製那個檔）。
📌 **讓守衛紅的代價不可以是開一個會付錢的窗口。**

**判準（可重用）**

> 寫任何「差集／缺口」工具時先問：**這件事有沒有第二條合法完成路徑**
> （補發、請款、人工結清、遷移結轉）？有 ⇒ 那條路的憑據也要進已完成集合。
> **共用判準 ≠ 共用資料**，而判準共用會讓人不去問第二格。
