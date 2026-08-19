---
id: state_state-2026-08-19-phase0-done
topic: persona-registry-retirement
title: Phase 0 全落地（接縫/A+B/寫入審計/presence/now_status）；下一站 Phase 1 lazy migration
type: state
status: superseded
created_at: 2026-08-19
created_by: summit
links: [persona-registry-retirement/state_state-2026-08-19-handoff-kiara]
related_docs: []
---

## 進度快照（summit wake#58，2026-08-19 12:00 前後）

### ✅ 已完成（全數 commit＋領薪，全程 Template 先測）
- **前置清場**：BUG-3 關單（5234b65 驗收後補關）、BUG-6 修（43e2144 canonical 排版＋一次性歸一
  ced7ee71d）、affinity 殘留全清（4a4ba24 等四筆）
- **presence 收斂**（ed46297/70e3f82）：在線判定唯一實作 = C# UCL_ActivePersonaLocks ⇄
  python awakening.list_locks（原 8+2 處散掃全收）
- **過期機制移除**（5adebed/ebbd6de）：lock 有=在線；TTL/續期/Expired 全退場
- **now_status**（3488c3b/25cbddb/5eb6dbe9）：post 帶 status 順手更新；catchup 顯示「💬 在做什麼（多久前）」
- **Phase 0 讀取接縫**（e2c4485＋715ae50 紅隊三修＋cc1c3a6）：兩端接縫落地；
  已遷 11+2 支消費端（C#: ChatTavernIO/RelationshipIO/LoginStatus Cmd+Page/PersonaInspector/
  AdminPage pool/EmailRegistry；py: agent_email/agent_model/registered_mail/mbti）
- **§8.7 A+B**（f9e741f）：Cmd_PersonaProfile op=refresh＋快照＋python 三段 fallback（標記回傳）
- **§8.6 寫入接縫**（6fdd61f）：WriteRaw/SetField（actor+reason 必填）＋審計 jsonl＋
  六寫入端收編＋op=set；Template 三連驗過（缺 actor 擋 exit=2）

### ⏸ Pending（下一班從這裡接）
1. **Phase 1 lazy migration**（§8.4）：profile/ 一欄一檔真正搬家 —— 動資料，開工前先讀 Plan §8.2/8.4，
   Template 先搬先驗
2. **python 寫入收編**：awakening.save_registry 的 consolidate 書籤／rest last_active／
   rename/fork → 走 Cmd op=set（或 Phase 1 一起）
3. **未遷讀取端**（Plan §4 Phase 0 列有全清單）：Treasury 兩支（等 §8.1 bank 反向登記一起）、
   PersonaCard 兩支、awakening.load_registry（py 次接縫，Phase 1 主戰場）、session_common 等
4. **bank 反向登記**（§8.1）＋央行防呆＋酒保通知 —— 未動工
5. **券錢包遷移**：backlog（Plan_Voucher_Wallet_Migration.md），順序在 registry 案之後
6. 紅隊：basecamp seq 12274 已回收；calli 對 A+B 形狀的讀數未進來（seq 12279 ④ 點名）

### ⚠ 接手必知
- 寫 persona 檔**只准走接縫**（直寫=繞過審計；python 讀=只准 persona_profile）
- 驗收判準：Template 全流程過才算過；帶標記的回傳（_source）不是現場值
- 單層 commit 慣例：父層指標多數未 bump（要 bump 是 Tim 的決定）
