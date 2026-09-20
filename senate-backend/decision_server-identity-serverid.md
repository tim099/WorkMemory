---
id: decision_server-identity-serverid
topic: senate-backend
title: Server 身分化（serverId）：main 逐字沿用舊路徑，其餘加後綴
type: decision
status: active
created_at: 2026-09-21
created_by: summit
links: [tavern-senate-migration/decision_d10-explicit-writer-switch]
related_docs: []
---

TASK-0244 交付（Senate `b0c2ef7`）：五樣東西帶 serverId ——
`_server_singleton.lock` / `ServerRoot`（queues 根）/ `_server_heartbeat.json` /
`_server_stop.request` / registry tag。

| | `main`（預設） | 其餘（例 `tavern`） |
|---|---|---|
| 單例鎖 | `_server_singleton.lock` | `_server_singleton.tavern.lock` |
| queue 根 | `runtime/server` | `runtime/server-tavern` |
| 心跳 | `_server_heartbeat.json` | `_server_heartbeat.tavern.json` |
| 停機請求 | `_server_stop.request` | `_server_stop.tavern.request` |
| registry tag | `senate_server` | `senate_server-tavern` |

## ⚠ `main` 逐字沿用舊值 —— 唯一的不對稱，而它是刻意的

全部一律加後綴的話，**升級當下正在跑的那顆舊 Server 會從新版 binary 的視野裡消失**
（它的 tag 是舊的、心跳在舊檔名）⇒ 新版判「沒有人在跑」而起第二顆 —— **那正是本單要防的事**。
⇒ 零遷移、零孤兒 queue、升級中途那顆仍然看得見。不對稱只活在 `ServerIds.Suffix` 一個方法裡。

## 新酒館 Cmd 怎麼接上去

`ServerDelegateCmd.ServerId` 是個 virtual，預設 `ServerIds.Default`。
酒館那支 override 成 `ServerIds.Tavern` **一行**就招呼到另一顆，⛔ 不用動任何呼叫端。

## CLI 面

`--id <serverId>` / `server list` / `server stop --all`（build 腳本走這條）。
⚠ `--id` / `--all` 必須登記進 `Program.cs` 的 `RejectUnknownFlags` 白名單 ——
沒登記的話旗標閘會在 `ServerCommand` 看到它們之前就 exit 2，
而錯誤訊息逐字是「這支子命令**不吃任何旗標**」，讀起來像設計上沒有這個能力。
