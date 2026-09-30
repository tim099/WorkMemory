---
id: knowhow_task-write-needs-cross-process-lock
topic: unity-senate-migration
title: 任務寫入搬家的關鍵是鎖：s_RmwLock 只鎖單一 process，SCP_FileLock 現成可用
type: knowhow
status: active
created_at: 2026-09-30
created_by: basecamp
links: []
related_docs: []
---

**任務寫入搬家的關鍵是鎖，而 SCP_Core 已經有現成的跨 process 鎖（2026-09-30 basecamp，TASK-0324 分析／0349）。**

- Unity `UCL_TaskIO` 的併發保護是 `s_RmwLock`：**只在單一 process 內有效**。所以 `SCP_TaskIO` 檔頭寫「整格搬或整格不搬」——寫入分到兩個 process，配號（`_index.txt` read-modify-write）會撞。
- Task 那一包（4,704 行）沒有主緒 only 的 Unity API（`Cmd_Task.cs:212`）；唯一綁 Unity 的是 UniTask。讀取那半已在 SCP_Core（`SCP_TaskIO`／`SCP_TaskModels`／`SCP_TaskReconcile`）。
- `SCP_Core/Runtime/Io/SCP_FileLock.cs`（TASK-0263）：OS 檔案握把做的跨 process 互斥，持有者死掉 OS 收回 ⇒ 沒有幽靈鎖；游標、佇列已在用。⇒ 寫入＋配號包進它 ⇒ Unity 與 Senate 兩邊都能寫，**可以不必經過 Server**。
- 接縫：任務的酒館公告（`UCL_TaskNotify` 借 Editor 發文 ⇒ TASK-0350）、work_memory 那段是 python ⇒ 走宿主注入（`SCP_ITaskCommitGateway` 那個形狀）。
- ⚠ 2026-09-30 下午看到 `ucl-task` skill 已改寫成「寫入走 `senate cmd task`、寫入端是 Senate Server」，SCP_Core 多了 `SCP_TaskStore.cs` ⇒ **可能已經有人做了**。接手前先查，⛔ 別重做。
- 順手量到（⛔ 未重現）：`op=create` 參數檢查失敗時號碼仍被吃掉（0351／0352 消失）⇒ 看起來先配號再驗參數。
