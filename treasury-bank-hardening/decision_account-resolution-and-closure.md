---
id: decision_account-resolution-and-closure
topic: treasury-bank-hardening
title: 帳號解析／歸戶／銷戶的三條拍板（順序・認字面判準・銷戶三閘）
type: decision
status: active
created_at: 2026-08-14
created_by: summit
links: []
related_docs: [ucl_core:Docs~/{lang}/Workflows/Treasury_Account_Consolidation_Workflow.md, ucl_core:Docs~/{lang}/API/UCL_AgentCommand/Cmd_Treasury.md]
---

Tim 2026-08-14 拍板：帳號解析可以先做，但「試跑前先 commit 金流相關檔案」。

**三件事的順序不可顛倒：止漏 → 標記遷移 → 歸零後銷戶。**
反過來做的話，搬完的當天孤兒又會長出新的。

**判準（決定 Credit/Debit 要不要歸一帳號）**：
> 「從既有帳號清單選出來的」＝認字面（resolveAccount:false）；
> 「從身分推導出來的」＝歸一（預設 true）。

所以轉帳單三處（出款/入款/回滾）與後台直接轉帳一律認字面。
🩸 理由是血的：歸戶轉帳的出款方**本來就是孤兒帳戶**，讓解析介入會變成
「從正主帳戶扣錢、孤兒原封不動，而轉帳單顯示核准成功」。

**銷戶三道閘**（Tim 逐條加的）：餘額 0 / 無 persona 綁定 / 非註冊帳號。
銷戶不是刪除 —— ledger append-only，帳戶只是「曾出現過的 account_id」，
所以銷戶只能是一份拒收名單，歷史一個字不動。

**後台整合而非另開頁**（Tim 問「或是整合在這頁面?」）：
registry 的寫入端已在 BankAdminPage，另開頁 = 同一份 JSON 兩個寫入者。
且 agent_banks 已有兩個寫入者（BankAdminPage 開戶 / PersonaAgentAdminPage 開 agent），
新區塊刻意讓它唯讀，不加第三個。
