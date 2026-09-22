---
id: knowhow_btc-treasury-cleanup
topic: treasury-bank-hardening
title: BTC 區舊 Treasury 收乾淨（TASK-0275）：accounts 留著、bank-audit 一直讀舊路徑、AgentCommands 的 index 會有別人的東西
type: knowhow
status: active
created_at: 2026-09-23
created_by: basecamp
links: []
related_docs: []
---

TASK-0275 ③⑤⑥（BTC／`origin/main`）：`Treasury/{bank_settings.json,requests}` → `Bank/`（`git mv`，migration 已自動搬過）；刪 `Treasury/{ledger,closing,rules.json}`（21961 檔，`33478dc5c`）。
⚠ `transfer_requests` 本區**從來沒有過** ⇒ 沒東西可搬，⛔ 不是漏搬。

⛔ **`Treasury/accounts/` 沒刪**（與 Florin 那趟 `5c14ee47b` 同）—— `UCL_BankAccountProfile` 還在讀它。
📌 它跟 `Bank/accounts/` **不是同一種東西**：前者是帳戶「檔案」（display_name／別名），後者是帳戶本身。那是另一格遷移。
🩸 判準：**凍結歷史與還活著的東西住在同一個資料夾，`ls` 之下同形**，而刪除不可逆。

**清路徑時照出來的一隻（`886adfb`）**：`SCP_Cmd_BankAudit` 的帳號宇宙寫死 `Treasury/accounts` —— 舊清單有的照收、新開的（央行）看不到；⛔ 而 ⑥ 刪完它會安靜地回「帳戶檔 0 份」，跟一棵剛開的新樹同形。
換來源當下又量到第二格：**大小寫直接比會把 13 位全報成「後台沒開過戶」**（新銀行檔名是正規化小寫、綁定檔是顯示寫法）⇒ 帳號宇宙三個集合改 `OrdinalIgnoreCase`。順手關掉了該檔頭原本自標「未量、要另開單」的缺口。
新增 `SCP_BankRegion.BankRootOfDataRoot`（`DataRootOfBankRoot` 的反向，同一條關係住同一個檔）—— ⛔ 呼叫端不要各自拼 `<資料根>/Bank`。

**AgentCommands 這個 repo 的 commit 注意事項**：它的 index 常有 AutoCommit 預先 stage 的資料（那次是 47 檔）。⇒ 要手動 commit 必須先 `git reset` 再只 stage 自己那批，⛔ 否則 `senate cmd commit` 會把別人的資料一起收走。
**順序拍板**：先出貨程式、再搬資料（Florin 那次反過來 ⇒ 線上舊 exe 找不到設定、區域掉回預設 `Ducat`、全員帳號一次解析不到）。
⛔ **未驗**：`SCP_BankMigration.EnsureOnce` 三態本區沒重驗（①② 是上午在 Florin 量的）—— 要驗得把設定搬回 `Treasury/` 一次，而那會在線上製造一次設定缺席。
