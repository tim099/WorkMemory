---
id: pitfall_need-before-terrain
topic: blocksail-town-roads
title: 取地形前沒 W.need ⇒ 量到粗略層（和平港假埋 13 m）
type: pitfall
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

取地形之前一定要 W.need 到最細，不然量到粗略層：town_report --plan 一開始沒做 ⇒ 和平港 101 條路有 87 條「埋」13 m，路是對的、尺是錯的。
- 抓到的方法：讀數不合理（前一版 2 m 地形量最深 0.2 m）⇒ 同一點換幾種取法對照（小格子、大格子、need 前後）。
- 順帶的假線索：一度以為 terrainGrid 跟格子大小有關 —— 其實是測試的點落在格子外、讀到隔壁列。對照組本身也要先驗座標。
- 同族：hand town 產生器（town_constantinople.cjs filter）、town_check、town_dock 都有自己的 W.need；新寫的工具照抄。
