---
id: state_state-2026-08-20-phase2-round1
topic: persona-registry-retirement
title: Phase 2 第一輪：§7② 關閉、BUG-19 開單並窄化（kiara）
type: state
status: superseded
created_at: 2026-08-20
created_by: kiara
links: [persona-registry-retirement/state_state-2026-08-19-phase1-done, persona-registry-retirement/state_state-2026-08-20-phase2-round2]
related_docs: []
---

## 進度快照（2026-08-20 09:0x —— kiara 接手 Phase 2 第一輪。**§7 驗收② 已關；BUG-19 開了並由 Tim 當場窄化**）

### 接手三步（不變）
1. `work_memory.py read --topic persona-registry-retirement --with-links`
2. Read `Plan_Persona_Registry_Retirement.md` —— **先讀 §4（分期／§4.1 殘留清單／§4.3 缺口）再讀 §8**。
   §7 表格下方新增「② 的量法與邊界」一小節，是這輪的讀數落點。
3. 四條鐵律照舊（讀寫只走接縫／Template 先測／`_source` 兩態不得同形／單層 commit＋改 .cs 必 recompile）

### ✅ 本輪完成（kiara 2026-08-20，全部唯讀量測，沒動 code）

**§7 驗收② 關掉了（⬜ → ✅）** —— 昨天我標「沒逐段核過，不冒充通過」的那條。
- 尺是 `wake_brief.py` 裡的字面空狀態字串（§1 `(尚無 fragment…)`／§3 `(未達門檻…)`／見林 `(尚無 digest)`）
- kiara wake#17 brief（573 行，tier-2）逐段：§0 血統＝`fork from crest-001`／
  §6.5＝calli 70・apex-one 57・gura 42 ＋ 2 篇 sketchbook 全文／見林＝全文 11 行
- 全庫：見林＋見人兩段都有讀數 **9/21**，其餘 12 位是資料真的沒到（見林要 10 夜），
  印的是帶讀數的明確空狀態句；`_RELATIONSHIP_LOAD_ERROR=None` ⇒ 4 位的「沒關係紀錄」是真沒有
- 兩個邊界寫進 Plan：①§0 血統行沒有空狀態文案（同形，但**沒有活的退化路徑** —— tier-2/3 對
  `forked_from` 0 人分岔，7 位真是根）②**② 不能拿 Template 當受測體**（它必然三段落空）

**Phase 2 首筆「消費端拿到舊值」實測命中 → BUG-19**
- identity 8 欄 × 21 人 = 168 格，tier-2（快照）vs tier-3（local-parse）**分岔 5 格、全部是 `email`**
  （對照：Phase 1 收工當天同一組 168 格「逐字相同」⇒ 分岔是「有人改過 profile/」之後長出來的，
   正是 §4.1 預測的形狀）
- 最毒的一格：gura 的 legacy `.email` = `basecamp05122026@gmail.com`（**別人的地址，格式完全合法**）
- 抽樣 5 人問 `build_trailer`：tier-3 時 4 人分岔，全部得到「屬於別人的合法地址」，零警告
- 根因：`resolve_email` 只回自己那套 source（persona-override/agent-default/…），
  **不傳遞接縫的 `_source`** ⇒ 標記存在，但沒有任何消費端把它端到人眼前

### ⚠ Tim 2026-08-20 當場窄化 BUG-19（照這個判準，別照我原本的框）
**本地備援在正常流程中不該被讀到** —— tier-3 只在 Editor 不可用時觸發，
而正常提交流程（公告領薪）本來就要 Editor ⇒ 唯一活路是
**fresh clone ＋ Unity 從未開過 ＋ `--no-announce` 或裸 `git commit` 觸發 commit-msg hook**，
**目前實務上不會遇到**。
⇒ 不擋 Phase 2、優先序低；**現在補 fail-loud ＝ 加一段永不觸發的 code**（＝§8.1 央行 fallback
刻意不做的同一條理路）。真要收，跟 Phase 3 移除 legacy 分支一起。

### ⏭ 下一位的待辦（優先序，已按本輪結果重排）
1. **Phase 2 繼續觀察**：要跨過一次全 persona 登入＋一次晚安＋一次發薪（2026-08-19 起算）。
   看的是「消費端會不會拿到舊值」；本輪已證明**分岔會長出來**，所以這條不是形式主義
2. §4.1 兩個 ⬜ 的 python 端（`session_common`、`tavern_catchup.resolve_owning_agent`）
   —— 都只讀 routing 不碰 identity ⇒ 今天不痛，收之前要確認呼叫時機是否在 Cmd 內（需 `UCL_PP_SKIP_CMD=1`）
3. **§8.1 央行 fallback ＋ 酒保通知**：刻意未做（同上理路），跟 Phase 3 一起
4. **Treasury 兩支**（`TreasuryAccountResolver` / `BankAdminPage`）讀 persona 檔那部分，跟正向鏈退場一起收
5. Phase 3 前置：pool 名單的新真相源（`persona_routing` 的 key 集合），Phase 2 之後才切

### 🔴 開放線／已知缺口
- **BUG-16**（open）`op=set` 寫不出 absent ⇒ 建議 `op=unset`
- **BUG-17**（open）接縫 module 載入三份。⚠ **本輪量到更狠的一格**：
  `agent_email._persona_profile()` 是**每次呼叫都 `exec_module` 一份新模組** ⇒
  module 級快取整個失效，不帶 `SKIP_CMD` 時是**每次 `load_persona` 一趟 Cmd**，不是「一個 process 三趟」。
  單子上的描述比實際樂觀，修之前先重量。
- **BUG-18**（open）§4.1 殘留清單本身
- **BUG-19**（open，已窄化見上）
- calli 對 A+B 形狀的紅隊讀數仍未進來（不擋工）
- 券錢包遷移＝backlog，順序在本案之後

### 🩸 這輪的血證
1. **`_local_parse()` 回的是 `{"personas": {...}, "pool": [...]}`** —— 我少剝一層直接 `.get(persona)`，
   拿到 `None`，於是量出「tier-3 掉血統 14 人／wake #0／agent `?`」**一整套假象**，
   而那套假象**看起來完全像一個重大發現**（合理、可怕、可解釋）。
   ⇒ 探針回 None 時 `dict(None or {})` 會安靜給你空 dict，**空 dict 餵進顯示函式長得像「資料掉了」**。
   量到「重大發現」的第一件事是回頭驗尺，不是寫報告。
2. **`_persona_profile()` 每次呼叫重 exec** ⇒ 對它做 monkeypatch 的探針**完全沒生效**，
   而兩組輸出「長得一樣」被我一度讀成「code 是安全的」。
   ⇒ **注入點要放在被呼叫端（`load_persona`），不是被重建的模組實例。**
   模擬失敗與「沒有問題」同形 —— 這是本命課在探針上的變體。
3. **改 CRLF 檔的 anchor 必須也是 CRLF** —— python 三引號字串是 `\n`，直接比對命中 0 次（這次是硬失敗，
   算運氣好）。手勢：讀寫都 `newline=""`，anchor 進 `rep()` 前統一 `\n → \r\n`，改完 `--numstat` 複驗
   （本次 26/5，行尾 570 CRLF / 0 bare LF）。
