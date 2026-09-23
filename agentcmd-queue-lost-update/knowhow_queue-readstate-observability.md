---
id: knowhow_queue-readstate-observability
topic: agentcmd-queue-lost-update
title: Load 的四態怎麼觀測（三條路的射程，含一臂根本沒有觀測面）
type: knowhow
status: active
created_at: 2026-09-23
created_by: kotoko
links: []
related_docs: []
---

`UCL_AgentCommandQueue.Load` 的 `oState`（Ok / Busy / Unreadable / Missing）**沒有直接的觀測面**，
三條路各自的射程如下（2026-09-23 逐格量過）：

1. **`ucmd run Invoke` 叫 `Load`** ⇒ 拿得到回傳值，**拿不到 `out` 參數** ⇒ 態讀不到。
   而 `Busy` 與 `Missing` 的回傳值都是空 queue ⇒ 兩者同形。
2. **Runner 行為**（`UCL_AgentCommandRunner.cs:315-318`）⇒ `!= Ok && != Missing` 才進 `aBusy` 分支
   ⇒ **`Busy` 會重新武裝 trigger、`Missing` 不會** ⇒ 這是唯一的行為觀測面。
   量法：獨佔握住 lane 的 `queue.json` ＋ 投 trigger ⇒ 逐次讀 trigger 檔，
   實測 `busyRetry` 1 → 2 → 3（0.3s／1.3s／2.2s）⇒ 3.2s 放棄。
3. 🔴 **而「真的不存在 ⇒ Missing」這一臂在本系統裡沒有觀測面**：
   `UCL_AgentCommandWatcher` 走 `ListAgentIds()`，而它是用 **`queues/<persona>/queue*.json` 列舉 lane**
   ⇒ **沒有 queue.json 的 lane 根本不會被派工**（實測：刪掉 queue.json 後投 trigger，25 秒沒有人來收，
   trigger 原樣留著）。⇒ 那條路自己另開了 TASK-0292。

📌 ⇒ 驗收條文要「爭用與真的不存在落在不同的態」時，**只有前半量得到**；
後半的誠實寫法是顯式標「無活體、憑據降為碼鏈（catch 順序：`FileNotFoundException`／
`DirectoryNotFoundException` 先接）」，⛔ 不要假裝它有讀數。

⚠ 觀測 trigger 時注意：Watcher 接手時會把 `pending.trigger` **改名成 `.running`**
⇒ 「trigger 消失」＝**被接手**，不是「這一批結束」。要量批次結束看 `.running` 消失。
