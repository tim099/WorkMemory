---
id: state_phase34-done-20260821
topic: persona-registry-retirement
title: Phase 3/4 收工：personas/ 改名封存，讀寫全遷 letters（11 支消費端）
type: state
status: active
created_at: 2026-08-22
created_by: basecamp
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Persona_Registry_Retirement.md]
---

# Phase 3/4 收工快照（basecamp 2026-08-21 夜）

## 做完了什麼
- **中央目錄退場**：`AwakenInit/personas/` 21 檔 `git mv` 成 `_retired_personas_20260821`（AgentCommands `e6d5c8e6f`）。
  **不是刪** —— 漏遷的讀取端會當場查無此人，而不是讀到過期值。
- **接縫改組**：`UCL_PersonaProfile.ParseLegacy` → `BuildPersonaRaw`（從 letters 組非 profile 欄）。
  `agent` ← `bank/<本專案區域>.md`（實測 21/21 與舊 agent 欄逐字相同）；
  `status`/`last_active` ← lock；`wake_count` ← `wakes/` 信數（在線 +1）；`last_consolidated_*` ← `longterm/` 檔名。
- **pool 判準**＝`letters/<p>/profile/` 存在（Tim 選項 A）。實測 21/21 真人有、12/12 幽靈目錄沒有。
  **掃到 0 位一律 LogError**（submodule 沒 init 會靜默少人 —— 這是選項 A 的明說代價）。
- **`model` / `actual_agent` 由 routing 轉 identity**（住 profile/）。三端清單同步；21 人已 lazy 遷移，
  其中 10 位的 `actual_agent` 是真的缺席（不生空檔）。`plurk_account` 同批納入（白天那筆）。
- **寫入端逐欄分流**：`WriteRaw` identity→profile／推導欄不寫但列進 `oError` 與審計／`agent` 拒收並指路 `op=set_bank`；
  `SetField` 對推導欄 fail-loud；`FreezeLegacyIdentity` 與 python `_freeze_legacy_identity` 退場；
  python `save_registry` 只寫 metadata、persona 半邊大聲丟棄，收到 identity 欄直接 SystemExit 指路 Cmd。
- **建人路徑**：先寫 `bank/<區域>.md` 再寫身分欄（先有帳號歸屬，錢才不會在半成品狀態落央行）。
- **路徑入口全拔**：`ResolvePersonaFile` / `PersonasDir` / `personas_dir()` / `persona_file()` /
  `tavern_paths.PERSONAS_DIR`（python 兩支改成 raise）。
- **python tier-③ 備援改讀 letters**（Editor 未開那條路）：實測 pool=21、basecamp agent=claude-code。

## 銀行那條線的遺漏（Tim 問「之前遷移有漏嗎」→ 量出來的）
資料遷移 08-20 就完成（帳本 `account-rename` 那批，此後 0 筆進 `-da-xiaojie`），python 端也早合一。
漏的是 **C# 對偶沒跟上**：
1. `UCL_AwakeningService.ResolveBankAccount` 還走 `agent_banks` 兩跳 ⇒ 解出不存在的 `claude-da-xiaojie`。
   已改一跳合一（**刻意不走 NormalizeAgent** —— `zeta`/`Zeta` 合一後是兩個帳戶且其一已銷戶），
   並拔掉 `<agent>-da-xiaojie` 的 derive。順帶修好 `Cmd_Sculpture` 兩處扣款帳號。
2. `UCL_TreasuryAccountResolver` 正向鏈建表直讀 personas/*.json ⇒ 改讀綁定檔；快取戳章改看 letters 根。
3. `UCL_BankAdminPage` 自己那份 persona→agent ⇒ 改走綁定。

## pending（下一個接手的人從這裡看）
- ⬜ `bank_personas` 反向表**是空的**（BUG-21 無寫入端）⇒ `Resolve` 的 ⑤-a 是死碼，實際走 ⓪ 合一。
- ⬜ `Cmd_GoodNight` 的 `aActor` 仍由 `ResolveBankAccount` 供給（公告 sender 與收尾信作者名）——
  函式已合一所以值是對的，但那條鏈還在。
- ⬜ `_lib/affinity_manager.py` 是唯一寫死 legacy 路徑的檔，全樹**零 importer**（可跟封存目錄一起處理）。
- ⬜ 封存目錄本身（Phase 4 的「刪」）：Tim 沒說刪，我沒刪。要刪走 git tag 備份，不留在樹裡。

## 血證（這次新增的）
- **判準錯了兩次都是探針抓的**（`Cmd_Invoke` 直打＋讀 Editor log 回傳值）：
  ① `IsCanonicalAccount` 答的是「registry 宣告過嗎」—— `claude-da-xiaojie` 宣告過但從未開戶（canonical=true）
  ⇒ 那版等於沒寫（「有擋下 ≠ 被該擋它的規則擋下」）。
  ② `GetAccountSnapshotPath` 是 `<id>.snapshot.json` 餘額快取、不是開戶紀錄 ⇒ 反而冤枉真帳戶。
  最後用 `UCL_BankAccountProfileIO.ListAccountIds()`。
- **編譯報告不是唯一證人**：clean compile 之後再去 `Library/ScriptAssemblies/*.dll` 搜新字串的
  **UTF-16 位元組**（第一次用 UTF-8 grep 得到「查無」—— 探針壞了不是程式沒編到）。
- **AutoCommit 會把呼叫前已 staged 的東西併進第一個群**（BUG-30）：我 `git mv` 完直接 `op=commit`，
  21 個改名落進 `[chat]` 那筆。⇒ 先 commit 或先 unstage，再跑 AutoCommit。
