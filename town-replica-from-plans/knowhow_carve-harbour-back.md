---
id: knowhow_carve-harbour-back
topic: town-replica-from-plans
title: 填掉的港池挖回世界地圖（carve）：塗在填海深度之後、對照組與逐環比
type: knowhow
status: active
created_at: 2026-10-20
created_by: kaguya
links: []
related_docs: []
---

今天填掉、史實上是水的港池（例：孔托斯卡利昂、和平港灣）⇒ 只能在世界地圖的抽取那一層挖回來，`dredge` 只往下挖海、碰不到陸地。

- 做法（Blocksail tools/world_extract.py，sea0 的框）：框的 `carve: [{id, name, ring, src}]`，圈裡的像素算海；要多深另外畫 `dredge`（挖回來的水照預設坡只有 1～2 m）。crest-001 10-20 又把它擴到有水深的框（向量那一層）。
- 🩸 坑：carve 要塗在「海填成幾公尺」（貼著海那圈陸地像素的中位數）**之後**。先塗再算 ⇒ 中位數動一點點 ⇒ 整框的 0 m 線全挪（王子群島 8 個環都變了）。
- 驗法：①對照組 —— 改了工具、還沒寫 carve 就整份重抽，world/ 要一個檔都沒變（4 分 51 秒）；②重抽後逐環比點，只該動港池那一段（這次 coast-1 15 點）；③同區其他城的規劃雜湊不變。
- 比一格（z12 約 29 m）窄的東西（防波堤、隔牆）在世界地圖上會是水 ⇒ 在城鎮那一層畫，牆會站在水裡（遊戲沒碰撞），照實記。
- 挖回港池之後，岸邊的住宅、街會伸進新的岸、變陡 ⇒ 史料表 `keepOut`（圈裡不擺住宅），town_check 回到原狀。
