---
id: pitfall_river-port-sail-in
topic: blocksail-europe-ports
title: 長河的港：從海上開進去才算驗過（Worker 沒挖河、粗格子吸走兩頭、港區外另拉一筆）
type: pitfall
status: active
created_at: 2026-11-03
created_by: pinnacle
links: []
related_docs: []
---

倫敦（TASK-1163）驗收只把船瞬移上泊位，Tim 實測大小船都開不進去、出港直線穿陸（TASK-1192 修）。三個原因、修法與量法：

- **找航線的 Worker（world/route_worker.js）以前沒掛等深線索引的 0 m 線** ⇒ 挖回來的河（泰晤士、塞維亞的河、IJ…）在 Worker 裡是陸地 ⇒ 一律被吸到河口外。已修（只開索引、setCuts；不掛 attachBathy）。⛔ node 工具都會掛 bathy ⇒ 命令列量不到這個坑，一定要進地球儀從海上開一次。
- **route.js 長航線粗格子把兩頭吸走** ⇒ tieEnds：那一頭其實是水就從吸到的點細找一段接上；nav_net 同招（TIE_KM）。
- **河道只有 10 m 線** ⇒ 地球儀要 吃水＋1.5＋誤差 4 ⇒ 最多 4.5 m 吃水。大船要 20 m 線（harbour_deep --depth 13）。
- ⛔ 港口 ±2.5 km 的港區一變深 ⇒ 生成器在河道正中擺外海泊位、堵住別的泊位（大船 3 ⇒ 1）；⛔ 整條陡岸 ⇒「市中心走不到港口旁的岸」。⇒ 港區那一筆不動、港區外另一筆（thames-deep：範圍蓋滿河面、shore 5／ramp 6,14,30、剪到泊位航道接外段那一點上游一點）。
- 量法：`tools/port_draft.cjs --town <id>`（每個泊位最多吃水、卡在哪條；遊戲照快取開、不夠才現場找 ⇒ 兩條取好的）。2026-11-03 全世界讀數：里斯本入港 5 m、聖地牙哥 10.1、阿姆斯特丹 8.6（大船進不去，main 上就有）。
- 待辦（沒開單）：nav_net 細修一律要 20 m、河口找不到就軟擋走沙洲 ⇒ 倫敦的長邊最淺 0.8／1.4 m，城對城不照快取。
