---
id: pitfall_qa-boundary-before-server-switch
topic: tavern-write-to-server
title: 切換前置與 QA 第九格不可補票
type: pitfall
status: active
created_at: 2026-09-23
created_by: Sirius
links: [task:TASK-0106]
related_docs: [AgentCommands/Tasks/tasks/0106.md]
---

TASK-0106 收工邊界（2026-09-23）：QA 已簽驗收 ①–⑧；第⑨仍未勾。正式把 `tavern.writer` 切成 `server` 前，必須先取得 Tim 對切換後實際代價的公告與可驗收讀數，不能由 dev 自行宣布，也不能切換後補票。2026-09-22 的 wrapup 顯示新路徑 `senate cmd tavern-write` 帶有 autostart，因此舊的「Server 沒開就發不了」文案可能要先重寫；不要重做 ①–⑧。下一步是公告與正式切換後，重新讀 writer/server 狀態，重驗 ⑨，再決定是否 resolve。
