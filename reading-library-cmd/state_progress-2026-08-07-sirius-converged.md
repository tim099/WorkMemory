---
id: state_progress-2026-08-07-sirius-converged
topic: reading-library-cmd
title: 進度快照：queue 根治收掉、1~4 與 Sirius 收斂、新前置=修 facts 假滿值
type: state
status: superseded
created_at: 2026-08-07
created_by: summit
links: [reading-library-cmd/state_progress-2026-08-07-day-end, reading-library-cmd/state_progress-2026-08-07-share-live]
related_docs: []
---

## 進度快照（2026-08-07 上午，取代 day-end 那份）

### 今天已收
- **queue 失敗不堵塞已根治**（UCL_Core `ddaf711` + AgentCommands `94a6041a`）：
  Runner 失敗自動出隊＋`_cmd_results/<id>.json` verdict 檔；run_cmd wait 改讀 result 檔
  （不再消失＝成功）。實測：炸彈 Invoke exit 2 + queue 0；DebugLog ✓ result 檔判定。
- 1~4 與 Sirius 收斂完（seq 10416/10417）——順序與閘見 decision_python-recall-retire-gate。

### 未做（新順序，開工等 Tim）
0. **【新前置】修 C# recall facts 假滿值**（見 pitfall_recall-facts-false-empty）——
   收斂時逐節點名補齊 C# 缺的三節（作品與媒材／書架投影／facts 全文）。
1. 發文整合：Cmd_Tavern internal post 回 seq → RecordSharedSeq；不自呼 WriteMessageWithSeq。
2. UCL_ReadingNotesManagePage 接 RenderRecall（Tim QA）。
3. Python library.py reading-recall **直接刪**（閘：facts 修完 + diff 只剩 generated_at）。
   閘關上前別交錯跑兩版 recall（同路徑互蓋，現在漂移方向是內容變少）。
4. op=scan / op=migrate：scan 第一個實測樣本 = readers/unknown/（別清）。

### 待辦（Sirius 撈的鄰居病，歸我 C# 側）
- ExportCmdSchema 每日節流 → source_hash 變更即重跑（schema 隔夜擋新 Cmd）。
- persona 大小寫寫入端 case-normalize（Linux 上會把追回檔寫到版控外）。

### 環境變更備忘
- WorkMemory 已被 Sirius 遷成共用 submodule（tim099/WorkMemory，AgentCommands `4ef7af29`）。
- 工具集新增 UCL_GitSubmoduleSyncPage / UCL_AutoCommitPage（與本主題無關但今天落地）。
