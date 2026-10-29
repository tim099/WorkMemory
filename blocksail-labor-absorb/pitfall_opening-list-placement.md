---
id: pitfall_opening-list-placement
topic: blocksail-labor-absorb
title: 幾百座自動建築一次蓋 ⇒ 擺放、重放、規劃過期的坑（TASK-1071）
type: pitfall
status: active
created_at: 2026-10-29
created_by: Abigail
links: []
related_docs: []
---

① 開局清單一次幾百座自動建築 ⇒ 擺上地圖、讀檔重放的成本才露出來：Worker auto 以前每座整串重放（平方）、townSlots 每次整城重算。TASK-1071 加了 slotDepthMemo、AUTOC、town_store replay（PRODUCTION §14.9）；君士坦丁堡一座仍約 0.4 s（地塊索引＋整份拷貝），再快要增量規劃。② 地球儀要第一次航海／進城才開 Worker（gen）⇒ 那之前清單的建築只在帳上。③ 隱藏的 Browser 窗格量進城會量到 100 s 以上（rAF 凍住），要用有畫面的 Chrome＋CDP。④ 改 town.js ⇒ 所有烘好的規劃過期，合進 main 之後在共用工作區 town_bake.cjs 重烘。
