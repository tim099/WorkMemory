---
id: pitfall_rmw-no-await-premise
topic: task-management-system
title: Cmd_Task 的併發安全靠「RMW 無 yield」這個沒有語法在保護的前提
type: pitfall
status: active
created_at: 2026-08-25
created_by: summit
links: []
related_docs: []
---

本檔的併發安全**來自「單一主執行緒 ＋ read-modify-write 中間沒有 await」，不是來自任何鎖**。
這裡刻意沒有鎖（TASK-0026，2026-08-25 實測 race 打不出來：三人同時 create ⇒ 3 檔連號零空洞；
兩人同秒 comment ⇒ 兩則都在）。

⚠ 會咬人的地方是這個前提**沒有語法在保護**：
`Cmd_Task` 的每個 Op 都是 `Require`(讀) → mutate → `Save`(寫)，而所有 `await` 都在 `Save` 之後。
今天任何人為了「先把記憶寫進去再記在單子上」把 `OpWrapup` 那個
`await UCL_TaskWorkMemoryCli.AddAsync`（內部是 `await Task.Run`，**本檔唯一真的離開主執行緒的地方**）
搬到 `Save` 前面，這隻就當場變活的，而症狀是**靜默的**：整檔覆蓋、留言消失、index 撞號，沒有一格會紅。

⇒ 已裝的告警：`UCL_TaskIO.Save` 進入時斷言主執行緒（只 LogError 不丟例外 ——
丟例外會把「前提破了」變成「使用者操作失敗」，那會逼下一個人拿掉斷言而不是修前提）。
⇒ 可 grep 的邊界：`grep -c "[RMW-END]"` 回 10，與 `Save` 呼叫點數相同。**新增 Op 忘了標，兩個數字就對不上。**

🩸 而「零 Task.Run」這個結論我曾經寫進交付報告，**它是錯的**（我只掃了 runner 與 watcher，
沒掃 Task 資料夾自己）。錯的證據留在紀錄裡會替將來的 bug 作證 —— 所以這裡寫清楚：
**有一個 Task.Run，它在 `UCL_TaskWorkMemoryCli.cs:74`，而它目前站在 `Save` 之後。**
