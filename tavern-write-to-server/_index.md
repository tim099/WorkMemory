# 工作記憶索引 — tavern-write-to-server

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_payroll-queue-for-later** — 發薪等不到銀行時排進它的 queue、不等

## pitfall
- **pitfall_autostart-heartbeat-window** — autostart 判上線與讀得到 build 之間有窗（＋Console 顯示的生成路徑與量法）  ↔ tavern-write-to-server/decision_payroll-queue-for-later
- **pitfall_qa-boundary-before-server-switch** — 切換前置與 QA 第九格不可補票  ↔ task:TASK-0106
- **pitfall_server-exe-rename-invisible** — 執行中 Server 的 exe 被改名：status 看不到它、它仍握著鎖
- **pitfall_wrapup-0106-202609230727** — 收工紀錄 TASK-0106：酒館 seq 與銀行 ledger 寫入端搬進 Server（第一支需要 Ser…
