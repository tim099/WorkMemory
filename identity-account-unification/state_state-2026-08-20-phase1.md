---
id: state_state-2026-08-20-phase1
topic: identity-account-unification
title: 階段一施工中：第 0／1a 步完工＋換區 round-trip 驗過（kiara）
type: state
status: active
created_at: 2026-08-20
created_by: kiara
links: [persona-registry-retirement]
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Identity_Account_Unification.md, ucl_core:Docs~/zh-Hant/Workflows/Bank_Region_Binding_Migration_Workflow.md]
---

## 進度快照（2026-08-20，kiara。**階段一施工中**）

### 接手三步
1. `work_memory.py read --topic identity-account-unification --with-links`
2. Read `ucl_core:Docs~/zh-Hant/Plan/Plan_Identity_Account_Unification.md`
   —— 拍板全集在最前面的引用區（⑥～⑮ 四批），施工表在 §5，人工拍板清單在 §4.2
3. 跨專案遷移照 `ucl_core:Docs~/zh-Hant/Workflows/Bank_Region_Binding_Migration_Workflow.md`

### 一句話的架構原則（Tim 2026-08-20，最重要的一條）
> **框架統一認 persona；其餘身分資訊（agent／bank／顯示名／email／model）
> 一律透過 persona 走統一解析入口。**

⇒ 任何「呼叫端自己算出 agent／bank／sender」的 code 都是待收的債。

### ✅ 已完成（全部實測，讀數在 Plan）
- **第 0 步**：`UCL_CentralBankSettings.CurrencyId`（key `currency_id`、預設 `Ducat`、
  檔名合法性守衛）＋ 後台「🪙 區域（貨幣）ID」面板（二段確認、寫入後讀回複驗）
  ＋「📂 開啟設定檔位置」。**本專案（LY）＝ `Florin`**
- **第 1a 步**：`UCL_LettersPath.BankDir/BankField` ＋ 接縫 `GetBankAccount`／`WriteBankAccount`
  ＋ `Cmd PersonaProfile` 五個新 op（`get_bank`／`set_bank`／`migrate_bank`／`rebind_region`／`unbind`，
  批次類**預設 dry_run**）＋ `UCL_AutoCommitRules` 收 `bank/` 群
  ＋ **21 位 `bank/Florin.md` 已落盤**（審計 21 行）
- **換區重綁**：後台改 ID ⇒ 自動四段（預檢／複製／翻設定／刪舊區），衝突即整批中止。
  Tim 實按 `Florin → BTC → Florin` **round-trip 零漂移**，審計鏈 42/21/21/21 一筆不差
- **BUG-22 兩半都修**：`Cmd_Tavern.ResolveDisplaySenderId`（顯示身分取綁定的 agent，不取 bank）
  ＋ `git_commit.py` 移除 `resolve_sender` 與 `sender_id` 傳遞（`d0af620`）。
  ⚠ 移除 sender_id 之後**計酬照常**：實測 `Myth` 2393 → 2399（＋5 commit ＋1 發文）

### ⬜ 下一批（優先序）
1. **第 1b 步**：Treasury 解析端接綁定檔、`Resolve()` ⑥ 分支改 `Debug.LogError` ＋ 落央行。
   ⚠ **不能就這樣接** —— 綁定值是 agent id，而錢還在舊帳號名下
   （LY `claude-code`=0 而 `cc`=884；Bar `claude-code`=17 而 `claude-da-xiaojie`=6,573）
   ⇒ 直接接＝薪水靜默轉向餘額 0 的合法帳號。走 Plan §5.1 的 (A)：先改名歸併再接。
2. **統一解析入口的剩餘六個呼叫端**（盤點在 `d0af620` 的 commit 訊息）：
   `dice.py`／`freetime.py`／`mbti.py` 三份 `_resolve_sender` **幾乎逐字相同**、
   `registered_mail.resolve_bank`、`canvas.resolve_agent_for_persona`（**碰錢**）、
   `library.py` 三個 post 呼叫點。
   正確形狀＝**讓呼叫端不必解析**：`awakening.tavern_post` 的 sender 參數改可省（python 端單一收束點）；
   canvas 那份要走 Treasury 統一入口而不是刪掉。
3. **階段二**（兩邊專案都確定之後）：`-da-xiaojie` 去除（10 帳號 8,808 token、**7 個撞名**）、
   撞名歸併（提案在 §4.2 D.2，**逐筆待拍**）、bank id → agent id 改名（ledger transfer）。
4. **Bar 專案跑階段一** —— 前置：Bar 的 UCL_Core 要 bump（實測停在 `ae7f7931`，沒有 bank 接縫）。

### 🔴 未驗／已知邊界
- `ambiguous` 分支（本區無綁定、其他區域 ≥2 個候選 ⇒ 拒絕挑選）**沒有實測** ——
  要造它得在別人的 letters 留垃圾。Bar 設好 ID 之後會自然走到。
- `identities.json` 內容已漂：`cc` 被登記成 `kind=agent`／`display_name=crest-001`，
  而現行 agent 名一筆都不在表裡。BUG-22 的顯示問題修了，**這份資料本身還沒整理**。
- 3 位在線 persona（basecamp／gura／meadow）的 `bank/Florin.md` 尚未 commit（自動 commit 預設跳過在線者）。

### 🩸 血證（會再咬人的）
1. **修「補值邏輯」前先數有幾個呼叫端顯式繞過它。** BUG-22 的 Cmd 端修好、驗收全綠，
   而 `git_commit.py` 顯式帶 `sender_id` ⇒ 最大的呼叫端整條路繞過去。
2. **受測體要選「兩個值不同」的人**（詳見 `persona-registry-retirement/knowhow_template-persona-as-test-subject`）。
   我的 bank 名＝agent 名（都 `Myth`）⇒ 修法在我身上永遠看起來完整。**同一形狀一天三次。**
3. **`--persona` 會被戳進 args** ⇒ Cmd 的篩選參數不可叫 `persona`（叫了會被那個宣告當篩選條件）。
   實測：letters 掃描從 9 個 repo 靜默縮成 1 個，輸出「repos=1」像探索 bug，不像撞名。
4. **`check_compile` 的兩來源對帳會救你**：有一輪 tracker 說 `errors=0`／`warnings=0`（前一輪 57），
   ErrorLog 同一時刻有 namespace 錯。**那盞燈不能單獨採信**；驗「code 真的編進去」的方法是**跑它**。
