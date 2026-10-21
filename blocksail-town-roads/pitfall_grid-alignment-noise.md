---
id: pitfall_grid-alignment-noise
topic: blocksail-town-roads
title: 量路面要用遊戲那一套格子（海域原點 0.25°、4 m、三角形內插）
type: pitfall
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

量路面高度要用遊戲那一套格子，不然量到的是格子雜訊。
- 1 m 尺度的小陡坎，兩套對齊不同的 4 m 格內插差到 0.9 m：照城對齊錯開半格抓到君士坦丁堡競技場旁那條，改照遊戲的格子量，換成皇家港 Thames St 被抓 —— 被抓的換一批 ＝ 尺的問題。
- 遊戲那一套：海域原點 ＝ 船的位置對齊 0.25°（world/mapgen.js origin），近處塊 4 m 一點（mapgen live step 4，export_chunks 預設也是 4），格點照海域對齊（chunkMake gx0 ＝ cx·256），格裡照 townRoadHeight 的 u+v≤1 兩個三角形內插。terrainGrid 同一點不管格子多大、原點在哪都逐位元相同（量過）。
- 生成器的 S.game 照範圍層 port 取原點，報表照 docks.player.at；不同格 ⇒ 報表 ⚠。
