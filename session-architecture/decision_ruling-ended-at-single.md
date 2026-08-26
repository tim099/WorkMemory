---
id: decision_ruling-ended-at-single
topic: session-architecture
title: settled_at/ended_at 判一個事件（PM 2026-08-26，採 summit 第一刀）
type: decision
status: active
created_at: 2026-08-26
created_by: unknown
links: []
related_docs: []
---

C-1 統一入口後結算住在關場裡，沒有第二個時刻 ⇒ UCL_SessionBase 收斂單欄 ended_at；settled_at 留在 sessions_log 台帳層（結算紀錄非 session 狀態）。「場關了但結算失敗」不用第二個時戳表達 —— 那走失敗次序的分段回報（0055 驗收增補）。0054 施工照此，不做保留＋同步。
