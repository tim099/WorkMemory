---
id: knowhow_annex-grid-auto
topic: blocksail-production
title: 附屬的田：4 m 格子、長方形自動蓋、量格子的邊
type: knowhow
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

附屬的田（Blocksail world/annex.js）的格子、試放、自動蓋：
- 格子 ＝ BSW.TOWN_CELL（4 m），對齊主建築的地塊、第 0 排臨路；地塊的邊不是 4 的倍數時，最後一排／欄只蓋到一部分，照樣算地塊的。
  - ⚠ 距離、後緣一律量「地塊佔的那一塊格子」的邊，不量地塊真的邊：皇家港 fort-rupert 14 × 14 m 最後一排伸出 2 m，照真的邊量，旁邊那一格比正後面那一格近。
- 自動蓋是一塊長方形（Tim 參考 Anno 1800）。挑法依序：
  1. 格數最多。
  2. 後面先於兩旁。
  3. 伸出自己那幾欄最少（第一版先比離中點 ⇒ 橫躺在鄰居後面，隔壁一格都蓋不出來）。
  4. 離中點近。
  5. 方的。
  - 路邊 12 m 先留給住宅，擺不下才放寬。
- 驗：tools/annex_check.cjs。
  - 已知答案要構造（例：先找連過去整片過得了的那一排、再拆橋）；拿城裡碰巧的地形當答案，四次都是測試錯。
  - 手擺的城（0840 之後不綁路）⇒ ① 沒得對 ⇒ exit 3，⛔ 不當成過。
- 一定要畫圖看形狀：讀數全過的那一版，形狀是錯的。
