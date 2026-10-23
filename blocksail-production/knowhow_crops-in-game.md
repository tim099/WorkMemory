---
id: knowhow_crops-in-game
topic: blocksail-production
title: 作物與田在遊戲裡：annex ⇒ 清樹、鋪土、種；貼圖集分張
type: knowhow
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

作物與田在遊戲裡（Blockside 16_treegen.js、63_trees.js、63_towns.js；分支 kaguya/crops 998b1c5）：
- 規劃的地塊帶 annex：{ cells, crop } ⇒ townPrep 的 T.fields ⇒ 清樹（併進 clear）、鋪土（townGroundTile）、種（treeFieldPut）。
  - crop ＝ TREE_SPECIES 的 key；place.field ＝ [行距, 株距]；place.soil ＝ 土色。
- ⚠ 土高出地面 TOWN_ROAD_LIFT（0.12 m）⇒ 作物要種在土上；第一版種在地面，菜畦整株埋在土底下。
- 遠景貼圖集一張最多 128 格（2048²），照 meta.region 分張、要畫到才拍。
  - 中途拍會改 GL 狀態 ⇒ 拍完要還原 program、clearColor、depth test。
  - ?bbpage=n 測拆張。
- 測試：export_chunks 的 --area 就是地圖資料夾名（不同名 ⇒「沒有給 BSMAP.area」）；在匯出的 towns.js 地塊上加 annex（scratchpad/cpl8/inject_fields.cjs），用有畫面的 Chrome、shots_f.cjs 拍。
- ⚠ 被殺掉的 admin.py build 會留下半寫的 index.html（25 MB）⇒ 重建，⛔ 不提交那一份。
