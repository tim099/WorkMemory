---
id: pitfall_rope-end-guess-by-distance
topic: constitution-broadside
title: 繩的端點照距離猜綁在哪，猜到旁邊的模型就跟著斷（哨兵號繩梯、憲法號 shroud）
type: pitfall
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

繩的端點「綁在哪」是照距離猜的（10_ship.js linkRopes：先錨在 5 格內的船殼、再找 3 格內的帆桁、4 格內的模型外框、最後才找繩）——
猜到旁邊的東西就會跟著它一起斷：哨兵號砲塔裡的繩梯，中間五道橫繩被判成綁在 XI 吋砲尾（4 格內），砲一被打掉繩梯就判成斷了、水手沒人爬（2026-10-17 量到才發現）。
basecamp 同一天也踩到同一族：憲法號 shroud 頂端離帆桁不到 3 格，被當成綁在帆桁上，轉帆時整條跟著轉。
修法（只修一角）：跟著砲塔轉、雕刻寫明 `rope:母繩/比例` 的繩只找繩；端點靠著砲塔的殼也算有靠（armNear）。根因（照距離猜）還在。
另：砲塔裡的繩要跟著轉 ⇒ 27_turret.js ropeRideOf 照 r.ends／r.frame 算兩端綁在哪一座；沒有綁在砲塔上的繩 ⇒ null、一行不多算。
量法：sh.ropes 看 dead；sh.nav.ladders 的 broken；把頂上砲班放倒（c.st=2）看 45 秒內補回幾個。
