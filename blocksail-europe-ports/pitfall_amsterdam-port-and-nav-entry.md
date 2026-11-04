---
id: pitfall_amsterdam-port-and-nav-entry
topic: blocksail-europe-ports
title: 阿姆斯特丹：港口點、重取格子、nav 入口點、先烘鄰城
type: pitfall
status: active
created_at: 2026-11-03
created_by: basecamp
links: []
related_docs: [docs/TOWN_TOOLS_TERRAIN.md, docs/WORLD_FORMAT.md, tools/nav_ports.cjs]
---

阿姆斯特丹（TASK-1164，2026-11-03）踩到的三個坑，塞維亞（長河、窗裡沒有 20 m 的海）會再遇到：
- 港口點要放在城那一側的水上：第一次放在車站島北邊的 IJ ⇒ 城前照舊 0.6 m、棧橋要伸 1 km ⇒ 換點重跑。harbour_deep 拿格子正中央當港口 ⇒ 換點就要重取格子。
- 重取格子前先 `git checkout HEAD -- world/bathy`：harbour_grid 的 --skip 只拿掉收礁的區，上一次彎進 world/bathy/<檔> 的線還在 ⇒ 會把自己挖的水道當天然深水（goal 一下就到）。
- nav_ports 的入口點：窗（港口 ±14 km）裡每一格都比 20 m 淺 ⇒ 以前取掃描順序第一格（窗的西北角），阿姆斯特丹落在 0 m 的死巷、尋路圖從那裡穿沙丘；9256b1ae 起改在改線的水道上挑。nav_ports 印「取最開闊那一格（0 m）」時，先看入港航線外段最淺幾公尺。
- worktree 跑 nav_ports 之前，窗裡別的城的規劃要先烘（town_bake --town <城>）：沒烘 ⇒ 棧橋不擋、入港航線跟 main 不同（合 main 遇到加的斯）。
- 局部等高線：DEM 唯一抽得出的 10 m 圈若是建築（阿姆斯特丹水壩廣場的王宮），那一框把 minKm2 調大濾掉；0 圈的框照「離岸 ÷（離岸＋8 km）」接到 10 m。
