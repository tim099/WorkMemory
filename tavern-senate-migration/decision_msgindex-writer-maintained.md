---
id: decision_msgindex-writer-maintained
topic: tavern-senate-migration
title: 訊息索引由寫入端維護，讀取端只讀
type: decision
status: active
created_at: 2026-09-30
created_by: basecamp
links: []
related_docs: []
---

**`_msgindex.txt` 由寫入端維護：Server 寫完一則就在房間鎖裡 `SCP_TavernMsgIndex.Refresh`；Editor 與 CLI 讀取端都只讀（TASK-0335）。**

- 以前維護者是 Editor 的讀取端（缺天就順手 Rebuild）⇒ 兩個 process、兩份格式規則寫同一個檔，而且索引新不新鮮取決於 Editor 有沒有開。
- `Refresh`：驗一遍，只有動過的那幾天現場列舉，整份寫回；索引不存在／不可信 ⇒ 全量重建，**每 process 每房只試一次**（舊格式房重建必放棄，不設上限就每寫一則付一次全量）。
- `Save` 是 tmp → `File.Replace`（讀的人讀不到半份）；兩個寫者（Server＋手動 rebuild）同時寫 ⇒ 最後的勝出，勝出的若是舊的 ⇒ 那天 mtime 對不上 ⇒ 現場列舉 ⇒ **答案仍對，只是慢一次** ⇒ 不需要跨 process 鎖。
- Unity `UCL_ChatTavernMessageIndex` 轉呼叫 SCP 之後要把路徑**接回 Editor 的 messages root**（取最後兩段重組）：讀取端的訊息快取用路徑當 key，`/` 與 `\` 不同就變成同一則兩份快取。Unity 那支 `Verify()` 用 Ordinal 比對就是在驗這一格。
- 還會落後的情況：繞過寫入端直接丟檔（遷移工具、人工）⇒ `senate cmd tavern-index --arg op=rebuild`。
