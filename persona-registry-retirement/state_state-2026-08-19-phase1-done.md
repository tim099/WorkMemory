---
id: state_state-2026-08-19-phase1-done
topic: persona-registry-retirement
title: Phase 0-1 ＋ §8.1 完工，Phase 2 觀察期進行中（kiara 收工快照）
type: state
status: superseded
created_at: 2026-08-19
created_by: kiara
links: [persona-registry-retirement/state_state-2026-08-19-handoff-kiara, persona-registry-retirement/state_state-2026-08-20-phase2-round1]
related_docs: []
---

## 進度快照（2026-08-19 17:45 —— kiara 收工。**Phase 0-1 ＋ §8.1 完工，Phase 2 觀察期進行中**）

### 接手三步（照這個順序，不用背）
1. `work_memory.py read --topic persona-registry-retirement --with-links`
2. Read `Plan_Persona_Registry_Retirement.md` —— **它已經從分析轉成施工紀錄**：
   §4 分期表是 ✅／🚧／⬜ 三態、§4.1 是「仍直讀 legacy 的消費端」清單、
   §4.2 附帶落地、§4.3 已知缺口、§7 驗收逐條標狀態。**先讀 §4 再讀 §8。**
3. 四條鐵律照舊（讀寫只走接縫／Template 先測／`_source` 兩態不得同形／單層 commit＋改 .cs 必 recompile）

### ✅ 本輪完成（kiara，全數 commit＋領薪、兩端各驗）

**Phase 1（identity 搬進 `letters/<p>/profile/`，一欄一檔）**
- 合併層做在 C# `GetRaw` 內部 ⇒ 消費端一支不改、快照免費繼承（`7621322`）
- `_field_sources` 三態 `profile / legacy / absent`；**只遷 legacy 真有 key 的欄、不生空檔**
- `FreezeLegacyIdentity`（C#）＋ `_freeze_legacy_identity`（python）⇒ **舊源只出不進**
- 白名單閘已拆（Tim：存取即遷移）⇒ **21/21 人已遷**（`deadc65`）
- 讀數：`profile 150 / absent 18 / legacy 0`；round-trip **168 格 0 不一致**；
  legacy identity 合併 sha1 `95f8a615…` **遷移前後逐字相同**
- 結構欄寫入通道：`op=set` 依欄名決定型別、parse/形狀失敗 fail-loud（`1f89740`）
- rename 必須搬 `letters/`，獨立 git repo 直接擋＋印手動 SOP（`289eae6`）
- `AutoCommitPage` 收 `profile/` 群（`277483e`）—— 21 人的 profile 已入版控（Tim 按的）

**§8.1 反向登記（bank_personas）**
- 資料 `5394fae1e`（初值由現況逐位導出，day-1 不改變任何人的錢）
- 解析 `f4d823f`（**python ＋ C# 同一筆 commit** —— two-end contract，只上一端會兩邊各解一個 bank 而都不報錯）
- 撞名＝拒絕解析不挑一個；退正向鏈**不准安靜**（python stderr／C# Trace 加 ⚠）
- 驗收：python parity 0 不一致；**C# SelfTest 62 通過 0 失敗**，③ 段每位 trace 都是
  `persona X → bank Y（§8.1 反向登記）`—— 沒有 agent 那一跳

**消費端收斂**（Tim：能走 CMD 的就走 CMD，備援只要支援 brief）
- `agent_email.load_persona` → 接縫（`4c0f568`）＝agent_model／git_commit／commit-msg hook 四端一起好
- `awakening.load_registry` → 接縫（`f8807c5`）；`save_registry` 加防護
- C# `PersonaAgentAdminPage` fork 來源、`check_letters_layout`、`sync_letters_gitignore`（`705b6ae`）
- C# spawn 的 python 一律帶 `UCL_PP_SKIP_CMD=1`（`38e73bd`）—— **必要條件不是最佳化**：
  不擋就是在 Cmd 裡面再排一個 Cmd，要等 120s timeout 才有人發現

### ⏭ 下一位的待辦（優先序）
1. **Phase 2 觀察期**要看的不是「遷完了沒」（legacy 當天就歸零），而是
   **消費端會不會拿到舊值** —— 清單在 Plan §4.1／BUG-18。
   ⚠ 還沒收的：`session_common`、`tavern_catchup.resolve_owning_agent`（都讀 routing 不碰 identity ⇒ 今天不痛）
2. **§7 驗收②**（`wake_brief` 三段有實際讀數）**我沒逐段核過** —— 不冒充通過，這條要補
3. **§8.1 央行 fallback ＋ 酒保通知**：刻意未做（正向鏈末端會 derive `-da-xiaojie`
   ⇒ 那條永遠不觸發，現在寫＝加一段沒有消費端的 code）。跟正向鏈退場（Phase 3）一起做；
   前置＝python 端補央行常數（C# 有 `UCL_CentralBankSettings`）
4. **Treasury 兩支**（`TreasuryAccountResolver` / `BankAdminPage`）讀 persona 檔那部分
   —— 跟正向鏈退場一起收，先改只會做一半
5. Phase 3 另一個前置：pool 名單的新真相源（`persona_routing` 的 key 集合），Phase 2 之後才切

### 🔴 開放線／已知缺口
- **BUG-16**（open）`op=set` 無法把欄位還原成 absent ⇒ 唯一復原是手動刪檔＝繞過審計。建議 `op=unset`
- **BUG-17**（open）接縫 module 被同一行程載入三份 ⇒ 不帶 SKIP_CMD 時 3 次 Cmd 往返
- **BUG-18**（open）§4.1 那份殘留清單本身
- calli 對 A+B 形狀的紅隊讀數仍未進來（不擋工）
- 券錢包遷移（`Plan_Voucher_Wallet_Migration.md`）＝backlog，順序在本案之後

### 🩸 這輪的血證（會再咬人的，別當故事讀）
1. **`check_compile` 的綠燈要比對 Timestamp** —— 一天被騙四次，每次都是十分鐘前的舊快照。
   根因是把 `run Recompile` 的輸出丟進 `/dev/null` ⇒「有沒有真的重編」沒人在看。
2. **乾淨的編譯不代表檔案只被改了你以為的地方** —— 用錯編碼寫回一支 .cs
   （多 BOM ＋ LF 變 CRLF、整檔重寫），Errors 照樣 0；抓到它的是 `git diff --stat`。
   ⇒ 改 UCL_Core 的檔一律 `encoding="utf-8"` ＋ `newline=""`，**且改完看 numstat**。
   ⚠ 各檔行尾不一致：`bank_resolver.py` 全 CRLF、`UCL_TreasuryAccountResolver.cs` 全 LF ——
   先量再改，anchor 字串要用對的行尾組。
3. **守衛本身也要被實測** —— 第一版 rename 守衛把待檢欄位掛在**新名字**底下，
   而 profile/ 在 `letters/<old>/` ⇒ 沒攔到、改名照樣落檔（真的生出 Template2.json）。
   **沒攔到的守衛跟沒有守衛長得一模一樣。**
4. **glob pathspec 會讓我把「掃描器看不見」讀成「世界上沒有」** ——
   `git ls-files "…/letters/*/profile"` 的 `*` 不跨 `/` ⇒ 回 0 筆，我一度回報「12 位沒入版控」，
   實際是全部都在。換不帶 glob 的量法才對。
5. **一個會在事情只做一半時就變綠的判準，比沒有判準更糟** ——
   §8.4 的「legacy 歸零」在遷移當天就達成，而那時 `agent_email` 還在拿舊值。
   所以我在 §7 ④ 明寫「legacy 歸零不代表消費端都跑在新結構上」。
