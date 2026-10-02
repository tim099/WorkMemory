---
id: decision_senate-sync-exe
topic: unity-senate-migration
title: senate-sync.exe：先同步再起 Server（同一二進位靠檔名分辨）＋同步頁掃描中排隊
type: decision
status: active
created_at: 2026-10-03
created_by: basecamp
links: []
related_docs: []
---

2026-10-02（Senate 7fad143，Tim 測過 ok）：跨日發券讓信件庫在同步前就髒 ⇒ 新增 senate-sync.exe（build.sh 把 senate.exe 複製一份＋senate-sync.lnk）。
- 決定：**同一份二進位、靠檔名分辨**（SyncWindowExe.IsCurrent 讀 Environment.ProcessPath；單檔 publish 下 Assembly.Location 是空字串）。沒帶參數 ⇒ 只開 SubmoduleSyncPage（堆疊只放這頁、無返回列）、不拉 Server、不佔常駐橋；帶參數 ⇒ 跟 senate.exe 一模一樣。子命令 `senate sync-window` 是同一個窗。
- SubmoduleSyncPage：掃描（含 fetch）中照樣能按操作鈕 ⇒ 排隊，收割後執行；排隊中 TopBar 只剩 Cancel（id submodule/queue-cancel）；設定變了再丟背景掃描、repo 換了自動取消。原本那條 WaitForExit 會凍窗到 fetch 結束。
- ⚠ 射程（量到的）：它只擋「開 senate.exe 時順手拉 Server」那一條。**任何委派 Server 的 Cmd 都會自己 autostart**（`senate cmd commit` 的公告／領薪就把 Tim 收掉的 main＋tavern 叫醒了）；而 Tim 的 prefs 裡 `server.autostartOnLaunch` 本來就是 false。Server 前一晚沒關的話，跨日發券照樣在同步前寫髒。根治方向（沒做，等 Tim）：同步頁容許「不重疊的 dirty」（git ff 本身允許）。
- 驗證法：沒有 Server 的 scratch cwd 先跑 `doctor` 當正向對照（會拉兩顆），收掉後再量同步窗 ⇒ 0 顆；排隊用 `ui --click` 驅動活窗，root 指 Bar、全部 submodule 排除＝空範圍。
