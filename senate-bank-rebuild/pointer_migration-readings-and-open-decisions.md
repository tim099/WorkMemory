---
id: pointer_migration-readings-and-open-decisions
topic: senate-bank-rebuild
title: 遷移範圍／具名的放棄／大小寫合併的前提，與下一步（先做常駐守衛，不是遷移）
type: pointer
status: active
created_at: 2026-09-14
created_by: basecamp
links: []
related_docs: [task:TASK-0209]
---

**C 段（遷移）依 Tim 2026-09-14 拍板延後** —— 「新銀行不急著上，先確保系統驗證 ok 再逐步遷移」。
以下是遷移要用到的讀數與已拍板的邊界，**⛔ 都是當天的快照，動手前一律重取**。

**遷移範圍**：只遷「有綁 persona 的帳戶 ＋ 央行」。
| | 帳號數 | token |
|---|---:|---:|
| 要遷 | 9 | 34,484 |
| 不遷（非零） | 20 | 8,530（**19.8%**）|
要遷的九個：`cc`／`zeta`／`Myth`／`FRS`／`Altair`／`g`／`a`／`Template`／央行 `pacific-standard-public-deposit-bank`。
⚠ 總量會漂：同一天早上量是 42,927、中午是 43,014（+87 是我自己賺的）
⇒ **凍結點必須是一個讀數不是一個印象**；C3 的守恆是**對照表不是等式**，差額要列得出每一筆。

**已拍板的丟棄**（⇒ 這些是**具名的放棄**，不是「差額對不起來」）：
- `discord:*` 三個帳號（97 token）—— 舊 bug 遺留，⛔ 不遷。
- `claude-da-xiaojie`(4636)／`Tim`(371)／`Codex`(246)／`Luna`(84)：**不管**，只處理有綁 persona 的。
  📌 `claude-da-xiaojie` 值得記一句：那個 persona **是有綁定的**（綁 `a`），
  而這 4,636 在一個**用 persona 名字長出來的帳號**裡 —— 那正是「開戶不顯式」那隻 bug 的最大一筆。
- `Federal Reserve System` → 新 id `frs`、DisplayName `FRS`（Tim 拍板）。

**大小寫合併**（4 組）：`antigravity`／`gemini-da-xiaojie`／`zeta`／`zeta-da-xiaojie`。
⭐ 當天量到**每一組恰好只有一邊有錢** ⇒ 合併是純加法、不需要仲裁。
⛔ **遷移當天要重驗這一句** —— 若某組兩邊都有錢就停下來問 Tim，不自己選一邊。

**還沒拍的**：C7（舊帳凍結後跨遷移點的查詢要不要做唯讀工具）／A7 打包路線。

**⇒ 下一步不是遷移，是把驗證變成常駐的。**
目前 `[SCP_JsonExtensionData]`／Server 排乾／單例鎖／銀行 B1-B6 的驗證**全是丟棄式探針** ——
它們沒有長在必經路上的守衛，下一個人改壞不會有任何一層喊。
我的建議：純函式那幾格（JsonExtensionData／BankId／Account／Ledger 含併發反向對照）收進 `selftest`；
Server 那兩格要起子行程、較貴也較 flaky，維持手動重現：
`senate cmd server-ping --arg sleep=60 &` 然後 `senate server stop`。
