---
id: knowhow_navmesh-block-reuse
topic: blocksail-world-globe
title: 導航網一塊一塊照用、城鎮規劃指紋只看碰到的、管線先後
type: knowhow
status: active
created_at: 2026-11-05
created_by: unknown
links: []
related_docs: []
---

導航網一塊 512 m 一塊照用（TASK-1200 第 8 格，main de03fc9f）＋ 城鎮規劃指紋只看碰到的（TASK-1196，main 43ad01d0）：
- navmesh_bake：每一塊的指紋 ＝ 同一套輸入（navInputs、navLocalKey）、框縮到那一塊外擴 9 km；紀錄在 _cache/navmesh_tiles.json（不放 world/：整個 world/ 會打包）；worktree 從共用樹抄 navmesh.bin 時一起抄它，不然過期的格整格重烘
- ⚠ 地形指紋最細只到世界格子檔（金斯敦一帶 2.8°、多佛 5.6°、博斯普魯斯 1.4°）⇒ 地形改了那一半省不到；範圍改了那一半才省（城鎮 ± 3 ⇒ 4 km：增量 9.4 分 vs 從頭 26 分）
- ⛔ 入口點一動（ports.js 換了）⇒ 先 nav_net、再 navmesh_bake、再 nav_net：通道照 net.js 的 raw 烘。11-04 晚先烘導航網 ⇒ 83 格已經過期、兩個測試被混，重做
- 城鎮規劃的 '#index'、'#bathy' 不是檔：值掛在 townInputs 回傳的清單 .loc 上；Worker 的 fileHash 同檔名只讀一次、城跟城共用 ⇒ 每座城不同的值不能走 fileHash。新的地方要算規劃指紋也得先查 L.loc，不然每份都當過期（而且不會報錯）
- 重烘時間（量的）：城鎮規劃 34 份 12～15 分、深海層 21 分、導航網從頭 26 分、nav_net 5～6 分、nav_bake --check 23 分
