---
id: state_state-2026-08-20-migration-done
topic: identity-account-unification
title: 合一遷移已執行完畢（LY）＋ 統一 API 收斂 ＋ 留給 Tim 的三件
type: state
status: superseded
created_at: 2026-08-20
created_by: summit
links: [persona-registry-retirement, identity-account-unification/state_2026-08-20-bar-migration-done]
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Agent_Bank_Unification_Migration_Workflow.md, ucl_core:Docs~/zh-Hant/Plan/Plan_Identity_Account_Unification.md]
---

## 進度快照（2026-08-20 傍晚，summit）—— **合一遷移已執行完畢**

### 接手三步（沿用 kiara 的，補一份新文件）
1. `work_memory.py read --topic identity-account-unification --with-links`
2. Read `ucl_core:Docs~/zh-Hant/Workflows/Agent_Bank_Unification_Migration_Workflow.md`
   —— **本次新增**。含前置四格／逐步驗收讀數／失敗回滾語意／阻擋對照表／
   **§6.5 LY 首航實跑紀錄**（真實讀數 ＋ 跑完才發現的三格），Bar 照它跑
3. 舊的區域綁定流程仍在 `Bank_Region_Binding_Migration_Workflow.md`（§4 已補方向更正）

### ✅ 已完成（全部實測，讀數在文件 §6.5）
- **遷移執行**：改名 4 組（`claude-code→cc` 7／`antigravity→a` 6／`gemini→g` 1／`Zeta→zeta` 1）
  ＋ `Federal Reserve System → FRS` 搬 **6,253**（同一 `tx_1384375f`）
- **驗收**：綁定檔 21 vs registry 21 **不一致 0**；C# resolver 實測全部「一跳到底，agent_banks 未參與」
- **解析模式開關** `account_resolve_unified`（`bank_settings.json`，預設 false）——
  C# `UCL_TreasuryAccountResolver` 與 python `_lib/bank_resolver.py` **兩端都讀**
- **`Cmd_PersonaProfile op=rename_agent`** ＋ `UCL_PersonaProfile.RenameAgent`（Cmd 與 UI 共用同一支）
- **`UCL_BankMigrationPage`**：試跑表／rename 欄位／兩段執行（改名＋切換是原子操作、失敗整批回滾）／
  同名合併／解析模式開關
- **帳戶資料一帳一檔** `Treasury/accounts/<agent_id>.json` ＋ `UCL_BankAccountProfileIO`
  ＋ 後台「🏷 帳戶資料」編輯區；遷移時自動建檔。目前 9 個（8 綁定 ＋ 央行），顯示名稱先套 id
- **統一 API**：`UCL_TreasuryLedger.GetAllBalances()`（批次餘額）、
  `UCL_TreasuryAccountResolver.ResolvePersonaAccount()`（persona→帳戶，**模式切換封在 API 內**）、
  `CloseAccount()`（closed_accounts 唯一寫入端）
- **硬規則入檔**：銀行操作含餘額讀取一律走統一 API（`UCL_TreasuryLedger` 檔頭 ＋ ucl-coding 四副本）

### ⬜ 留給 Tim 按的三件（我刻意不自己按）
1. **同名合併 4 組**（`zeta`/`Zeta`、`zeta-da-xiaojie`/`Zeta-da-xiaojie`、
   `gemini-da-xiaojie`/`Gemini-da-xiaojie`、`antigravity`/`Antigravity`）—— **零搬錢**，遷移頁按鈕
2. **銷戶**：`Federal Reserve System`（已掏空）、`Fed`（為遷移復戶、現在用不到）
3. **`agent_banks` 退場** —— 5 筆仍是舊映射，已不參與解析；刪前先確認 `account_resolve_unified=1`

### 🐛 未解：BUG-25（Tim 19:04 抓到）
渲染端顯示仍是舊壞資料（`crest-001@basecamp`），而**訊息落盤的 `sender_name` 已正確**。
⇒ 渲染端自己重查 `identities.json`。候選 `UCL_DiscordIdentityResolver.cs:53` / `UCL_ChatTavernIO.cs:990`。
修法方向：優先用訊息內既有的 `sender_name`；要重算就走與 Cmd_Tavern 同一順序。

### 🩸 血證（給下一個接手的人）
1. **合一的方向由成本決定**：agent 名那側餘額全是 0 ⇒ 改 agent 名零 ledger 異動；反向要搬 11,338。
2. **「已改名但未切開關」兩條鏈都不對** ⇒ 改名與切換必須是同一個動作，失敗整批回滾。
3. **大小寫**：`zeta.json` 與 `Zeta.json` 在 Windows 是同一個檔 ⇒ id 唯一性以全小寫比對，開戶已擋。
4. **版控**：`Treasury/.gitignore` 原本整行 `accounts/` ⇒ 真相源放進去會**完全不在版控裡**，
   而 `git status` 對 ignore 的檔是安靜的。現在只 ignore `accounts/_*`。
5. **`Draw*` 裡只准讀記憶體** ⇒ 對 40 個帳戶現場查餘額會讓後台卡一分鐘、IMGUI 拋 layout 例外。
6. **同一件事只能有一個算點** ⇒ 我修了寫入端卻留下渲染端（BUG-25）、
   後台也曾自己寫第三份 persona→帳戶解析。**修別人的第二份時，先數自己有沒有第三份。**
