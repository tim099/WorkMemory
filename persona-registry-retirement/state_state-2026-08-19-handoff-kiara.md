---
id: state_state-2026-08-19-handoff-kiara
topic: persona-registry-retirement
title: 後續交接 kiara（Tim 派單）：Phase 1 lazy migration 起手，含接手三步與四條鐵律
type: state
status: superseded
created_at: 2026-08-19
created_by: summit
links: [persona-registry-retirement/state_state-2026-08-19-phase0-done, persona-registry-retirement/state_state-2026-08-19-phase1-done]
related_docs: []
---

## 進度快照（2026-08-19 12:30 —— **本案後續交接 @kiara**，Tim 派單）

### 接手三步（照這個順序，不用背）
1. `python <UCL_Core>/Tools~/AgentCommands/work_memory.py read --topic persona-registry-retirement --with-links`
   → 開它印出的 ReadBrief（pointer 有文件地圖、decision 有八條拍板索引）
2. Read `Plan_Persona_Registry_Retirement.md` 的 **§4（分期＋已遷/未遷清單）與 §8（拍板全集）** ——
   §1-§3 是 calli 的原始分析，改方向的部分以 §8 為準
3. 開工前把 summit 的四條鐵律讀進反射（見下）

### ✅ 已完成（summit，全數 commit＋領薪，全程 Template 先測）
- Phase 0 讀取接縫：`UCL_PersonaProfile.cs` ⇄ `_lib/persona_profile.py`；已遷 11+2 支消費端
- §8.7 A+B：`Cmd_PersonaProfile op=refresh`＋快照（C# 只寫）＋python 三段 fallback
  （live 無標記／snapshot 帶 `_source`+`_snapshot_at`／local-parse 帶標記）
- §8.6 寫入接縫：`WriteRaw`/`SetField`（**actor+reason 必填**）＋審計
  `AwakenInit/_persona_write_audit.jsonl`＋`op=set`；六個 C# 寫入端已收編
- 連動已完工：presence 收斂（UCL_ActivePersonaLocks / awakening.list_locks 唯一掃描）、
  過期機制移除（有 lock＝在線）、now_status（post 帶 status 順手更新）
- 紅隊：basecamp seq 12274 兩洞四題全數回收（含 email 歸位 identity）

### ⏭ 妳的待辦（優先序）
1. **Phase 1 read-through lazy migration**（§8.2/§8.4）：letters/<p>/profile/ 一欄一檔真正搬家。
   規則：存取時有 profile/ 新資料以新為準、缺就當場遷；**新值絕不回寫舊 personas/**；
   lazy migration log 記「觸發了誰的哪一欄」。實作位置＝接縫兩檔內部（消費端不動）。
   **先搬 Template、全流程（登入→讀欄→寫欄→晚安）過了才碰真人。**
2. python 寫入收編：awakening.save_registry（consolidate 書籤/rest last_active/rename/fork）
   → 改走 `Cmd PersonaProfile op=set`（或 Phase 1 一起做）
3. 未遷讀取端（§4 Phase 0 列有全清單）：Treasury 兩支（**等 §8.1 bank 反向登記一起**，
   別先動）、PersonaCard 兩支、session_common、check_letters_layout、sync_letters_gitignore
4. §8.1 bank 反向登記＋央行防呆＋酒保通知（獨立塊，可先可後）
5. 觀察期驗收照 §7（bank 解析 **python/C# 兩端各驗**）

### ⚠ 四條鐵律（violate 任一條 = 本案的病自己長回來）
- persona 檔**讀寫只走接縫**：python 讀=persona_profile、寫=op=set；C# 讀=UCL_PersonaProfile、
  寫=WriteRaw/SetField。直讀直寫＝繞過單端解析與審計
- **Template 先測**（Tim 拍板）：每批功能改好先拿 Template 全流程過，真人不當白老鼠；
  Template 走與真人完全相同的流程（不改名不排除）
- 回傳帶 `_source` 標記＝非現場值，**兩態不得同形**；顯示要換算「多久前」
- 單層 commit（父層 bump 是 Tim 的決定）；改 .cs 必 recompile＋check_compile

### 🔴 開放線
- calli 對 A+B 形狀的紅隊讀數未進來（seq 12279 ④ 點過名）—— 不擋工，進來要回收
- 券錢包遷移（Plan_Voucher_Wallet_Migration.md）＝backlog，順序在本案之後
