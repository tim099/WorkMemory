---
id: state_2026-08-20-bar-migration-done
topic: identity-account-unification
title: Bar 專案合一遷移完成＋解析模式開關移除
type: state
status: active
created_at: 2026-08-21
created_by: summit
links: [identity-account-unification/state_state-2026-08-20-migration-done]
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Agent_Bank_Unification_Migration_Workflow.md, ucl_core:Docs~/zh-Hant/Plan/Plan_Identity_Account_Unification.md]
---

## 進度快照（2026-08-20 深夜，summit）—— **Bar 專案合一遷移已完成並驗收**

### 接手三步
1. `work_memory.py read --topic identity-account-unification --with-links`
2. Read `ucl_core:Docs~/zh-Hant/Workflows/Agent_Bank_Unification_Migration_Workflow.md`（LY 首航紀錄在 §6.5）
3. Bar 的差異全在下面「Bar 與 LY 不同的地方」

### ✅ Bar 已完成（全部實測讀數）
- **區域 ID `BTC`**（Tim 確認沿用）；`migrate_bank` 落地 **21/21** 份 `bank/BTC.md`，
  我另外逐檔比對 `registry.agent` —— **不一致 0**
- **方向＝最終帳號 id 就是 agent 名**（Tim 拍板；不是 LY 的短名 cc/a/zeta，也不是預設的「agent 改名」）
  ⇒ 三組走**帳號改名**（真的搬錢，全部併入既有同名帳號）：
  `Zeta-da-xiaojie→Zeta` 3496／`antigravity-da-xiaojie→antigravity` 1463／`claude-da-xiaojie→claude-code` 5792
- **大小寫同名合併 3 組**（Tim 按）：`Antigravity`／`Gemini-da-xiaojie`／`zeta-da-xiaojie` 進 closed_accounts
- **一帳一檔 10 個**（只建最終帳號，不動孤兒）；顯示名稱從 identities roster 搬進來
- **解析模式開關 `account_resolve_unified` 已整個移除**（Tim：徹底改用新流程）——
  C# `UCL_CentralBankSettings` 與 python `bank_resolver.py` 同批拔，合一是唯一模式

### ⚠ Bar 與 LY 不同的地方（別套用 LY 的記憶）
1. **Treasury/.gitignore 是舊版**（整行 `accounts/`）⇒ 一帳一檔會完全不進版控。已修成 `accounts/_*`。
   **兩棵樹是不同 repo，LY 的修法不會跟過來。**
2. Bar 的帳號名帶 `-da-xiaojie` 後綴，LY 早就是短名 ⇒ **方向與成本都不同**
3. `pacific-standard-public-deposit-bank` **就是央行**（公庫＝央行，Tim 確認）

### 🩸 血證（會再咬人的）
1. **央行帳戶不在「帳號宇宙」的任何一個集合裡** ⇒ 每次撥款都喊「孤兒帳戶」。
   修法不是去 registry 補登記（那是第二份資料），是讓 `CentralBankAccount` **依定義納入 canonical**。
2. **守衛用 `IsCanonicalAccount` 判「能不能設為央行」會把舊央行鎖在門外** ——
   現任央行之所以 canonical 正是因為它是央行。改判「在不在帳號宇宙裡」（＝下拉能選到的就能設）。
   ⭐ 抓到它的是 **round-trip**（換過去再換回來），不是再讀一遍 code。
3. **`Encoding.UTF8` 會吐 BOM** ⇒ python `json.load` 直接炸。無 encoding 參數的多載反而無 BOM。
4. **BuildPlanReport 的 rename spec 與 Cmd_Invoke 都用 `;` 分隔** ⇒ 多組 rename 無法從 CLI 試跑，只能在頁面填。
