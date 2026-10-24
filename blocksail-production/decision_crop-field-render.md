---
id: decision_crop-field-render
topic: blocksail-production
title: 田裡的作物：照塊分組、不投影子、3D 300 m、放大 1.8（菜 2.25）
type: decision
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

田裡的作物怎麼畫（TASK-0903，2026-10-24 Tim 拍板；Blockside 63_trees.js 檔頭「田裡的作物怎麼畫」）：
- 照（田 × 16 m）分塊、塊裡照（作物, 第幾棵）排、存成實例緩衝的樣子；treeBin 只照塊的外框挑距離（50 m 內細的、其他低模），整段複製。
- 田裡的作物不投影子（陰影圖只畫樹）；野生的作物照舊。
- 3D 距離 TREE_FIELD_VIEW = 300（＝照各作物自己的 view）：100 m 那一版量不出差、100～250 m 的菜畦變一片綠（貼圖讓葉子蓋住八成五的土）—— Tim 回「好」⇒ 留 300。
- 大小：TREE_FIELD_K、TREE_FIELD_SP 1.5 ⇒ 1.8（Tim「整體再大一點(1.2倍) 然後密度再低一些」）；菜畦 place.fieldK 1.25（「菜可以大1.5倍」）。
- 讀數：君堡城內中畫質 60 ⇒ 216 fps；之前作物佔約 10.8 ms，其中約 8 ms 在每格重畫的陰影圖。
