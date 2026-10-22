---
id: decision_git-on-cli-side
topic: senate-repo-sync
title: 同步的 git 一律在 CLI 那一邊；Server 不跑 git（Tim 選 3）
type: decision
status: active
created_at: 2026-10-22
created_by: Abigail
links: [senate-repo-sync/pitfall_server-git-defender]
related_docs: [senate:Docs/Workflows/Task_Management.md, senate:Docs/Workflows/Repo_Sync.md]
---

Tim 2026-10-22 選「3」：同步的 git 一律在 CLI 那一邊跑，Server 不跑 git（Senate 063ca4b；BookNotes／WorkMemory 從一開始就在 CLI）。

- 任務單：sync.tasks 開著時，`task` 寫單整套由入口（Cmd_Task）在本行程做 —— 拉 → 寫 → commit → push → 撞號重放，全在任務板的跨行程鎖 `SenateTaskRepoSync.TaskLock`（Global 具名 mutex；Server 經 WMI 起，不保證同一個 session）裡。結果用 `Cmd_TaskWrite.BuildResult` 包，入口後面的通知／記憶不分兩條。
- Server 的 `task-write` 收到寫入（舊入口、Server 內部派發）⇒ `WriteWithoutGit`：只寫檔、回 pending，下一次 CLI 同步收成一筆（沒有操作紀錄 ⇒ 撞了只能交給人）。
- Server 每分鐘拉的背景工作拿掉，改成 `tasks` 讀單之前拉（每分鐘最多一次、鎖最多等 5 秒）；`task-sync` 本地跑。
- 閱讀筆記／工作記憶：`SenateFolderSync` ＋ `FolderSyncSpec`（加一種＝加一個 spec 成員＋設定一格＋一支手動 Cmd）；掛勾在 CLI：library／book、work-memory／task。

⇒ 新增任何同步或重的子行程工作：放 CLI，⛔ 不放 Server 的程式路徑。
驗收讀數：探針單 TASK-0825（tags=probe）由本機 CLI 建、結，4668d33／710a980 推上 tim099/Tasks，ls-remote＝本機 HEAD；同步開著時 main 存活、Defender 0 次。
