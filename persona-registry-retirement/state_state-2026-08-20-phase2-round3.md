---
id: state_state-2026-08-20-phase2-round3
topic: persona-registry-retirement
title: Phase 2：§7② 關閉 ＋ §4.1 python 清空 ＋ BUG-17 修完（kiara）
type: state
status: active
created_at: 2026-08-20
created_by: kiara
links: [persona-registry-retirement/state_state-2026-08-20-phase2-round2]
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

### ✅ 第二輪追加（kiara 2026-08-20，§4.1 python 端清空）

**`_lib/session_common.py` 整支刪除** —— 不是收進接縫，是它沒有存在理由了：
它是「上班模式全面退役」（`4f48884`）時為了不讓 `stream_watch_session.py` 壞掉才抽出的工具層，
而那支唯一消費端已於 `842801e` 退場（陪看改走 C# `Cmd_StreamWatch`）。
全樹 grep 零 .py／.cs 呼叫端；state 檔 `work_sessions.json` 連檔都不存在。
⇒ 對齊 Tim 2026-08-20 的話：**session 相關只有 Editor 開著才能跑，狀態擁有者是 C#／Cmd**。
連帶消失的是它自帶的正向鏈 bank 解析 `_resolve_bank(agent)`
（與 §8.1 反向登記今日實測 **0/21 分岔** —— 反向表初值由現況導出，
 它會在「銀行端改了誰屬於誰」那一刻才開始說謊）。

**`tavern_catchup.resolve_owning_agent` 收進接縫** ＋ **移除 `PERSONAS_DIR`**
（留著＝邀請下一個人再走直讀，同 `agent_email.persona_path()` 的移除理由）。
呼叫時機兩種都實測：① CLI 直跑 ⇒ `source=live`（走 Cmd 拿現場值）
② 模擬被 wake_brief 載進 Cmd 內部（`UCL_PP_SKIP_CMD=1`）⇒ `source=snapshot`、不再排第二個 Cmd。
21 人 agent 值與 legacy **0 不一致**；不存在的 persona 回 `''` 不拋；接縫模組**只載一份**
（刻意快取 —— 不重演 BUG-17 每次 `exec_module` 的寫法）。整支 catchup 端到端跑過，在線清單正常。

**靜態證明重跑（§5.2 手法①）**：`personas_dir|PERSONAS_DIR|PersonasDir|AwakenInit/personas`
全樹 py/cs —— **python 端沒有任何讀取端還直指 legacy**（只剩接縫／路徑解析器／寫入端 awakening.py／註解）；
C# 剩下的命中都有主（BankAdminPage 刻意擋、PersonaAgentAdminPage 是寫入端、
PersonaInspectorPage 只用路徑開檔案總管、ChatTavernAdminPage 是 pool 名單＝Phase 3 卡點②）。
⇒ Plan §4 Phase 3 卡點① 已縮到「刻意擋著的 C# Treasury 兩支」。

**本日 commit（單層，父層 pointer 仍指舊 hash）**：
`95d54f4` UCL_Core（Plan＋刪 session_common）／`00050f1` Tools（catchup 收進接縫）／
`c0bf0a9` WorkMemory／`912d8ec` BugReports（BUG-19 開單）／
`3c60944` UCL_Core（**BUG-17 修法：`_lib/seam.py`**）／`25024a2` Tools（catchup 委派 seam）。
六筆全部已領薪（`commit_payout_check` 逐筆對過）。

### ⏭ 下一位的待辦（優先序，已按本輪結果重排）
1. **Phase 2 繼續觀察**：要跨過一次全 persona 登入＋一次晚安＋一次發薪（2026-08-19 起算）。
   看的是「消費端會不會拿到舊值」；本輪已證明**分岔會長出來**，所以這條不是形式主義
2. ~~§4.1 兩個 ⬜ 的 python 端~~ ✅ **2026-08-20 收完**（見上，一刪一收進接縫）
3. **§8.1 央行 fallback ＋ 酒保通知**：刻意未做（同上理路），跟 Phase 3 一起
4. **Treasury 兩支**（`TreasuryAccountResolver` / `BankAdminPage`）讀 persona 檔那部分，跟正向鏈退場一起收
5. Phase 3 前置：pool 名單的新真相源（`persona_routing` 的 key 集合），Phase 2 之後才切

### 🔴 開放線／已知缺口
- **BUG-16**（open）`op=set` 寫不出 absent ⇒ 建議 `op=unset`
- ~~**BUG-17**~~ ✅ **已修並關單**（`3c60944` ＋ Tools `25024a2`，2026-08-20）。
  復現讀數比單子更狠：`agent_email._persona_profile()` **每次呼叫**都 `exec_module` 一份，
  且 `sys.modules` 裡零筆（`module_from_spec` 不註冊）⇒ 快取根本無法共用。
  修法：新增 **`_lib/seam.py`** —— 以**解析後的絕對路徑**當 `sys.modules` 的 key
  （不用人工模組名：名字靠多端同步義務維持，打錯字會靜默分裂成兩份）。
  ⇒ **以後要拿接縫一律 `seam.persona_profile()`**，不要再自己 `spec_from_file_location`。
  驗收：三消費端同一實例、key 恰好 1 筆、警告 3→1（Template 實跑回傳檔）。
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
