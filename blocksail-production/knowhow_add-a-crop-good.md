---
id: knowhow_add-a-crop-good
topic: blocksail-production
title: 加一種作物貨（薑）：land.species、奢侈品權重、會被碰紅的兩支檢查、田的植株
type: knowhow
status: active
created_at: 2026-10-30
created_by: unknown
links: []
related_docs: []
---

加一種貨（TASK-1109 薑）要一起看的：
- 開場檔 land.species 只記有配方用得到的物種 ⇒ 新配方要的物種要 land_start --write 1 補上，不然自動建築不知道那裡長得了（第一次跑一年產量 0）。巴拉科阿那一筆 land 整個是空的（TASK-1094），別順手寫進去。
- 4 級奢侈品照錢分一籃：多一種就分薄別人 ⇒ FOOD_LEVELS[4].w 給權重（薑 0.5）；量改前、權重 1、權重 0.5 三組。
- 兩支檢查會被新貨碰紅：prod_sim「奢侈品只花剩下的預算」原本站在進位邊上（預算 0.1）；jobs_check「房租讓金庫比只加崗位多」10-27 公共工程之後量不到房租 ⇒ 改成餘額＋works.cash。修尺之後改前、改後兩邊都要量。
- 田裡的植株：Blockside 16_treegen.js TREE_SPECIES 加在最後（前面每一種的種子不變；dev 那一份對新的網格雜湊比）；田裡一塊幾百棵 ⇒ 頂點數壓到菜畦那一級（細 ~600／低 ~170）。
- FARM_CROPS 不加 ⇒ 開場規劃不變；加了 ⇒ 長得了的城開場擺那種田（要 Tim 決定）。
- 看田：測試 worktree 套新核心（admin.py shell）⇒ 先重烘那座城、再改烘好的規劃 annex.crop／recipe（指紋過期的話地球儀會現算、改了白改）。
