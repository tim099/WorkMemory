# 工作記憶索引 — unity-senate-migration

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_senate-sync-exe** — senate-sync.exe：先同步再起 Server（同一二進位靠檔名分辨）＋同步頁掃描中排隊
- **decision_unity-keeps-only-project-features** — Unity 只留 Unity 專案功能；關場在 Senate 就地做

## knowhow
- **knowhow_retire-measure-successor-gaps** — 拆 Unity 一族前先量接班人缺哪一格（四次撞到藏在族裡的共用物）
- **knowhow_task-write-needs-cross-process-lock** — 任務寫入搬家的關鍵是鎖：s_RmwLock 只鎖單一 process，SCP_FileLock 現成可用
- **knowhow_task-writer-moved** — 任務單寫入搬到 Senate：形狀與搬『寫』時會撞的三格（TASK-0349）

## pitfall
- **pitfall_approve-all-double-pay** — 一鍵批准全部重複付款：兩條各自冪等的路合起來不冪等
- **pitfall_buildsh-failure-leaves-server-down** — build.sh 失敗不會把停掉的 Server 拉回來；它也會把別人的半成品一起編進去
- **pitfall_delete-feature-family-checklist** — 刪一整族 Unity 功能：先 grep 引用分類、刪完重編、殘留 using 不會當場報錯
- **pitfall_retire-cmd-not-single-writer** — Cmd 退場不等於單一寫入端：Unity 頁面自己也寫
- **pitfall_scp-gui-explicit-key-and-reload** — SCP_Gui 頁：explicit key 不吃 IdScope、迭代中途別 Load、下拉要世代號
- **pitfall_server-reporoot-is-senate** — Server 傳給常駐工作的 repo 根是 Senate 自己的 repo
