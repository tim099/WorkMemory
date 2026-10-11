---
id: decision_new-op-replayable-by-old-exe
topic: sculpture-senate
title: 新的寫入 op 要舊 exe 重播得出來：記成 workedit＋action，參數放最上層 source
type: decision
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

新的寫入類 op 要讓**沒更新的 senate.exe 也重播得出來**，就不能用新的 op 名字：重播（SCP_SculptStore.ApplyEvent）照 op 名字分支，不認得就丟「不認得 op=…」。
做法（TASK-0495 噴漆）：事件記成 `op=workedit`、`edit.action=<新動作>`，edit 裡**只用舊版 SCP_SculptEdit 就有的欄位**（action／target_event／persona／before／after／credits）—— 舊版 Populate 對 edit 裡不認得的鍵會報錯。
新動作自己的參數與讀數放在事件**最上層** `source`（CommitEdit 的第三個參數）：重播與歷史索引只讀 edit，不會碰它。undo／redo／草稿全走既有的路。
驗法：git worktree 切到上一版 SCP_Core，寫一支 console 重播器（參照那份 csproj）刪快取重播新版寫的事件、逐格比；反向對照把 op 改成新名字 ⇒ 舊版要失敗。
草稿 apply（action=draft）是同一個做法的前例。
