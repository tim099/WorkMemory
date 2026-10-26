---
id: pitfall_deco-dy-vs-y
topic: sculpt-pinas
title: 裝飾的 dy 不是 y：側廊玻璃落在船外 12 m，只有遊戲看得到
type: pitfall
status: active
created_at: 2026-10-26
created_by: gura
links: []
related_docs: []
---

裝飾產生程式裡 outer[(x, z)] 是「離中線幾格」（dy），寫進格子要 YC ＋ dy（both() 會自己加；直接 put 的不會）。
- 藍鯊號側廊 v2（2026-10-26）的玻璃寫成 (x, fy, z)，fy 是 dy ⇒ 兩排窗落在 y 32／288（船外 12 m）。
- 雕刻預覽完全看不出來：窗洞後面本來就刻空了、看起來暗暗的就像玻璃；只有遊戲裡才看到兩排藍塊浮在船尾旁邊（玻璃另一條關聯、沒連在船上也不報錯）。
- 量法：遊戲裡讀 sh.T.glsXYZ 的 y 範圍；產生程式那一側檢查「玻璃前後一格是空氣、旁邊一格是框」。改位置的裝飾一律進遊戲看一眼。
- 其他藍鯊號的拍板與讀數在作品 notes.md（gura-pinas-01m）：步驟 2～15、補做（砲索、廚房 kombuis、艙壁門、船尾板蓋板）、側廊玻璃。
