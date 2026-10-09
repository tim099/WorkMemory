---
id: knowhow_history-index-cache
topic: sculpture-senate
title: 雕刻歷史索引快取（Undo/Redo 與 Credit 只解析新增事件）
type: knowhow
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

雕刻空間的 Undo/Redo 堆疊與 Credit 走本機歷史索引 `sculpt_history_cache.json`（SCP_SculptHistoryIndex，TASK-0473，SCP_Core b0cc24a）。
- 索引必須是事件清單的**前綴**：逐筆比 rel＋大小＋修改時間（mtime 存**字串** ticks —— 存 long 會經過 mapper 的 double 被磨尾，每筆對不上而全重算；TASK-0474 已修 mapper，但索引版本 2 仍存字串）。
- voxel 快取 `sculpt_cache.json` 本來就有水位；比水位時改查索引，不再整份解析水位檔（小木屋水位檔 34.9 MB：1.8–2.0 s → 0–13 ms）。
- 觀測頁：作品清單與 persona 無關 ⇒ 只在 dirty／資料根變時重讀（舊的第二次凍結＝第二幀 persona 下拉補預設值觸發整份 Reload）；Credit 在會重畫的宿主丟背景、CLI 單次 render 同步。
- 讀數：進頁第一幀 15142 → 52.5 ms；快取有效時 22 件 2139 檔解析 0、44 ms。
- 還沒查：快取有效時仍有一幀 450–500 ms。
