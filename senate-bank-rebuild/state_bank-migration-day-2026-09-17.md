---
id: state_bank-migration-day-2026-09-17
topic: senate-bank-rebuild
title: 新銀行接手前要知道的五格（遷移日落地狀態）
type: state
status: active
created_at: 2026-09-17
created_by: basecamp
links: []
related_docs: []
---

新銀行（SCP_Bank）今天從「測試用」變成「有真錢、有雙寫、有對帳」的狀態。接手前要知道的五格：

① **銀行根是推導值** `<AgentCommands 資料根>/Bank`（Tim 2026-09-17「不額外設定」）——
   `bankRoot` 那個可填欄位已退場。⇒ 要換位置只能換資料根。
② **一區一本帳**：區域名讀舊系統那一格（`Treasury/bank_settings.json` 的 `currency_id`，本樹＝Florin）。
   ⚠ Bar 專案是另一棵樹（BTC），它的遷移**還沒做**；做之前要先切 Senate 的啟用專案，
   否則 `cmd bank` 會指著 LY 那本帳，**而兩邊都不報錯**。
③ **雙寫鏡像不在寫帳路徑上**：`UCL_BankMirror` 是 cursor pull（掛 Editor 的 mirror daemon 每秒 tick），
   逐筆發 `senate cmd bank`。失敗時 cursor 不動、退避 30 秒、印警告 ⇒ 漏送可補，⛔ 不連累舊帳本。
④ **射程是推導不是清單**：本區有 persona resolve 得到的帳號（**含借用別區綁定**）＋ 央行。
   🩸 我曾把「借用」讀成「不屬於本區」而把 @kaguya 在用的 `Luna` 銷戶 —— 借用＝在本區真的開戶並綁定。
⑤ **對帳器**：`ucmd run Treasury --arg op=bank_diff` 逐戶並排，⛔ 不印總差額（兩個方向相反的錯會互相抵消）。
   現況：本區正式帳戶 10 戶逐戶零差額；射程外 19 戶（錢仍在舊 Treasury）。

⚠ 權威**還沒切**：「我有多少錢」的答案仍是舊 `Treasury/`（D27）。切換閘是 TASK-0216 ⑧⑪。
