---
id: pitfall_tavern-queue-0372-traps
topic: tavern-senate-migration
title: 0372 排隊：淨室走不到 not_running／只停 Server 不夠／mv 還原不重編／新態別落進預設分支
type: pitfall
status: active
created_at: 2026-10-02
created_by: basecamp
links: []
related_docs: []
---

TASK-0372（酒館 Server 不在 ⇒ 發文排進它的 queue）做的時候踩到、換人接手要知道的四格：

1. **在線上 Server 開著時寫「Server 不在」的淨室 selftest，走不到 not_running。** 把 `ServerDelegateCmd.RepoRootProvider` 指到 temp 之後，`ServerHost.Probe` 仍從登記表看得到那顆**真的**酒館 Server 活著，而 temp 根裡沒有它的心跳 ⇒ 實際走的是 `build_unknown`（等 5 秒）。排隊機制驗得到（寫的是 temp 的 queue，安全），但 not_running／autostart 那幾條要靠「停酒館 Server＋放 `SenateData/runtime/_build_in_progress.flag`」對 exe 活體驗。⛔ 別把那格 selftest 讀成 not_running 的讀數。
2. **只停 Server 不夠**：`tavern-post` 會自己 autostart 把 Server 拉起來，走不到排隊。要重現 build 視窗得**同時**放旗標（BuildGuard 擋 autostart）。驗完記得刪旗標、`senate server start --detach --id tavern`。
3. **紅測之後用 `mv` 還原原始碼 ⇒ mtime 比 obj 舊 ⇒ 增量編譯不重編**，跑起來還是被改壞的那份，看起來像「還原了還是紅」。還原後一律 `touch` 再 build。
4. **新增 `SCP_TavernPostOutcome` 的一態時**，讀它的地方逐一看：小歇的 switch 有 `_ =>` 預設分支，新態會安靜落進「確定沒發」（然後叫人補發）。Unity 端 `UCL_TavernSenatePost` 也要認新的 `🔢` 標記，否則「exit 0 沒 seq」會被改判成 exit 7。圖書館 share 在沒有 seq 時叫人「重新 share」——排隊與 alter 排程都會因此發兩次（已修，3f7ec6a6）。
