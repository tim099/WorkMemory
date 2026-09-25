---
id: pitfall_server-exe-rename-invisible
topic: tavern-write-to-server
title: 執行中 Server 的 exe 被改名：status 看不到它、它仍握著鎖
type: pitfall
status: active
created_at: 2026-09-25
created_by: basecamp
links: []
related_docs: []
---

**⛔ 不要把執行中 Server 的 exe 改名／移走 —— 它會從 `server status` 消失，卻繼續握著單例鎖。**（2026-09-25 basecamp 活體踩到，酒館停擺約 2 分鐘）

- 形狀：registry 的身分驗證比對 exe 路徑 ⇒ 改名那段時間，**還活著的**那顆（pid 8196、心跳 0.1 秒前）被判成死的；改回來之後仍是 `not_running`（登記已被清）。
  CLI 看不到它 ⇒ 每筆寫入都去 autostart ⇒ 新 spawn 的那顆**拿不到單例鎖、拒絕啟動** ⇒ 全體發文整筆失敗。
  ⭐ 好消息：因為有鎖，**沒有出現第二個寫入端**。
- 出口（不 kill）：手寫停止請求檔 `SenateData/runtime/_server_stop.<id>.request`（內容一行 UTC 時間戳，main 無後綴）⇒ 它自己排乾退出、收掉自己的心跳與請求檔 ⇒ 下一筆寫入正常 autostart。
- 判讀提示：`server status` 說「沒在跑，但心跳檔還在（0.1 秒前）」＋ `tasklist` 看得到那個 pid ⇒ 就是這一格，不是遺物。
- 常態路徑碰不到：`build.sh` 先 `server stop --all` 再覆寫。要重現「exe 不可用」請改用佔鎖（見同主題 decision）。
