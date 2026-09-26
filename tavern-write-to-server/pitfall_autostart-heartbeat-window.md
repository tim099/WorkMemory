---
id: pitfall_autostart-heartbeat-window
topic: tavern-write-to-server
title: autostart 判上線與讀得到 build 之間有窗（＋Console 顯示的生成路徑與量法）
type: pitfall
status: active
created_at: 2026-09-26
created_by: basecamp
links: [tavern-write-to-server/decision_payroll-queue-for-later]
related_docs: []
---

**Server 先登記進 registry、後寫心跳檔 ⇒ autostart 判「上線」與「讀得到 build」之間有一個窗（TASK-0304，Senate `9d73fd9`）。**

- 窗裡 `ServerHost.Probe` 的 `Heartbeat` 是 null（或上一顆的遺物）⇒ 舊碼 `BuildMatches=false` ⇒ 報 build_mismatch、「這一筆沒有送出」、而 0297 刻意不排 mismatch ⇒ 發薪直接丟掉。血證：09-25 seq 21827（`server-tavern/_cmd_results/20260925-184449-c60590-tavern-write.json` 逐字 `Server build=?`）。
- 修法：`ServerStatus.BuildKnown`（心跳是**活著那顆 pid** 寫的且有 build）；不是 ⇒ `WaitForBuildKnown` 輪詢最多 5s；還不是 ⇒ `delegate_failure=build_unknown`，`ShouldQueueForLater` 收它。⛔ 同 pid 而 build 真的不同 ⇒ 照舊 mismatch、不排。
- ⚠ 「還不知道」與「確定不符」處置相反（再等 ／ 拒絕）⇒ 別再共用一個出口。
- Console 顯示（2026-09-26）：三條生成路徑（委派 autostart／ServerAdminPage 啟動／`server start --detach`）全走 `ServerSpawn.TrySpawn`；WMI 那條靠 `Win32_ProcessStartup.ShowWindow` 顯式 0/1。量「有沒有視窗」的讀數：顯示＝窗類別 `PseudoConsoleWindow`（被轉交 Windows Terminal；有時窗掛在 WindowsTerminal.exe 名下而不是 server pid），不顯示＝`ConsoleWindowClass` visible=False。雙擊 senate.exe 不顯示是**用 CREATE_NO_WINDOW 重生自己**（`ConsoleHost.TryRelaunchWithoutConsole`），因為 WT 接手後 `HideConsoleWindow` 藏的是看不見的那顆。
