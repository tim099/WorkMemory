---
id: state_20260814_account_resolution
topic: treasury-bank-hardening
title: 2026-08-14 帳號解析上線・銷戶已實點・遷移待點
type: state
status: active
created_at: 2026-08-14
created_by: summit
links: [treasury-bank-hardening/state_20260804]
related_docs: []
---

**2026-08-14（summit wake#51）帳號解析／歸戶／銷戶**

⚠ **先更正上一份交接的數字**：舊紀錄寫「8 個漂移帳戶約 226 token」。
實查 12,742 筆 ledger：**35 個孤兒、合計 2,616 token**，差四倍以上。
而且 `Zeta`(310) / `Fed`(114) 當天仍在增加 —— 那不是歷史殘帳，是還在漏的洞。
（2026-08-04 那輪歸戶把孤兒壓到 1,713，十天長回 2,616。）

**已落地且驗過**
- `UCL_TreasuryAccountResolver`（新檔）：六段解析，查不到就標 Unresolved 原樣通過（不 mint 不丟棄）。
- `Credit`/`Debit` 接上解析 + `resolveAccount` 開關；Debit 的 caller 也一併歸一
  （只歸一一邊會讓每筆合法扣款被判成盜用，而那個例外訊息長得像資安事件）。
- 轉帳單三處 + 後台直接轉帳改認字面。
- `SelfTest()` 六條**與資料無關的不變式**，56 項全過。跑法：
  `run_cmd.py run Invoke --arg type=...UCL_TreasuryAccountResolver --arg member=SelfTest`
- 端到端過收銀台：`op=debit account=TEMPLATE` → ledger 寫 `Template`，未生出新孤兒。
- BankAdminPage 新增兩區：👻 孤兒帳戶（標記遷移／銷戶）、🧭 帳號解析規則（試算／別名／補登記／復戶）。
- **Tim 已實點銷戶兩次成功**：`Gemini-da-xiaojie`、`antigravity-da-xiaojie-da-xiaojie`。

**已知資料狀態（不是 bug，但要有人看過）**
- 撞名 1 筆：`claude-da-xiaojie` 既是 system_account（5,463 token）也是 persona（agent=antigravity）。
  解析讓帳戶贏；固定列在 SelfTest ⑥。

**pending**
1. 「📝 標記遷移」按鈕**尚未有人點過**（銷戶點過了，遷移還沒）。33 個有餘額的孤兒等標記。
2. `agent_aliases` 仍是空的 —— 認不出來的孤兒（`zeta-bank` / `discord:xxxx` / `subconscious-daemon` …）
   要逐個決定「加別名搬走」還是「補登記原地承認」。
3. code 未 commit（單層 UCL_Core）；文件已寫：
   `Docs~/{lang}/Workflows/Treasury_Account_Consolidation_Workflow.md`（新）＋ Cmd_Treasury.md 同步 ＋ index 回填。
4. Python 端 `_lib/bank_resolver.py` **未與 C# 解析器對齊**（兩套規則，未對拍）。
