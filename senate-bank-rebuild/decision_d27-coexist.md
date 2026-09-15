---
id: decision_d27-coexist
topic: senate-bank-rebuild
title: D27：新舊銀行長期並存，遷移前新銀行＝測試用（取消 C8）
type: decision
status: active
created_at: 2026-09-15
created_by: basecamp
links: []
related_docs: []
---

Tim 2026-09-15 拍板（落成 ADR **D27**）：`SCP_Bank*`（Senate 原生）與 Unity 側 `UCL_TreasuryLedger`
**完全獨立、長期並存**，而且**遷移前新銀行一律視為測試用，「我有多少錢」的權威仍是舊系統**。

⇒ 這條**取消了 TASK-0209 的 C8**（「全部消費端切到新系統，必須與 B 同一次上線」）。
C8 的理由是我寫的：「只接 Server 而 Editor 仍直寫磁碟 ＝ 兩個寫入端，比現在更糟」——
⚠ **那句話的前提是兩邊在搶同一筆錢**。前提換掉之後它就不成立：兩套各寫各的帳本，不是同一本帳。
⇒ Editor 側 `UCL_TreasuryLedger` 全庫 **86 處／26 檔**（Credit 29／Debit 17／GetBalance 24）**一行都不動**。

**未來遷移的射程（拍板當天就寫死，⛔ 不是等到那天再想）**：只搬開帳金額（一戶一筆 `opening_balance`）／
只遷有綁 persona 的帳戶 ＋ 央行／舊 ledger 不刪不搬。→ 獨立單 **TASK-0216**（backlog）。

**並存期唯一的新風險與處置**：「我有多少錢」有兩個答案而**兩邊都不會報錯**（各自都對，只是在回答不同問題）
⇒ `Cmd_Bank` 的**每一次輸出**蓋一行定語「遷移前新銀行＝測試用，實際以舊系統為準（D27）」。
⚠ 遷移那天要回來把 `Stamp()` 拿掉 —— 它屆時會變成一句過期的真話，而**過期不會叫**（已寫進註解）。
