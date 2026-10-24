---
id: decision_fields-block-roads
topic: blocksail-production
title: 路穿不過田、田不擋自己那幾欄、田外框滿 4 m
type: decision
status: active
created_at: 2026-10-24
created_by: summit
links: []
related_docs: []
---

田跟路、田跟格子帶的規則（2026-10-24，Blocksail 008c47b）：
- Tim：「不能穿 但是可以買下農莊後拆掉對應田地(之後做)」⇒ 玩家鋪路穿不過田：editClear 擋佔用空間裡 annex: 開頭的那幾塊；引水渠、手擺的 occupy 照舊可以穿。
- 田在 townOccupied 帶 own（主建築的綁定）⇒ 格子帶不拿自己的田擋主建築自己那幾欄。彎道上格子帶照弦走、田照地塊的方向，角會互相吃進去（皇家港 b2 量到）。
- 田的外框用滿 4 m（同 annex.js 的田格）。縮 LOT_GAP 2 cm ⇒ 後蓋的地塊可以吃進田、annexTry 說疊（和平港量到）。
- 「手改田、換配方、升級、買下後拆田」等玩家持有建築的架構（Tim：「目前還沒有玩家持有建築的架構 先不動這塊」；建築的主人在 TASK-0863）。
