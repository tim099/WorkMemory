---
id: pitfall_route-water-in-fingerprints
topic: blocksail-world-globe
title: route.js、water.js 在烘焙指紋裡：玩家行為放 globe.html，尋路圖缺邊補法
type: pitfall
status: active
created_at: 2026-11-07
created_by: Abigail
links: []
related_docs: []
---

route.js、water.js 都在烘好快取的指紋裡（route.js ⇒ nav_net、nav_ports；water.js ⇒ 海圖塊 CHUNK_CODE、導航網 NAV_CODE_FILES、深海層 DEEP_CODE_FILES、城鎮規劃 townInputs）—— 改一個字就要重烘。
- 只給玩家的行為放在 globe.html（inlandWater／riverAt／riverOut／etaOf／netGo，2026-11-07 Blocksail 9b60983d）；非改 route.js 不可時，用烘焙工具不帶的 opt（例：opt.clear 只有地球儀給 ⇒ nav_net、nav_ports 內容逐字相同、只換指紋），還是要跑 nav_ports ⇒ nav_net 一輪、ports.js 拿內容 diff 證明只換指紋。
- 尋路圖（world/nav/net.js）照商船的小時挑路、候選只取直線最近 3 個 ⇒ 會缺邊繞一大圈；2026-11-07 加 nav_net ③b（圖上 ＞ 1.3 × 那一對的避陸航線就補，855d31dd，補了 8 條）。
- 重烘順序 nav_ports ⇒ nav_net ⇒ navmesh_bake（新邊的通道）⇒ nav_net，約 45 分；合進 main 之後 world/navmesh.bin、_cache/navmesh_tiles.json 從 worktree 抄回共用樹，navmesh_bake --check 核。
- 量航線彎不彎用「每 200 m 一點、前後各 1 km 的航向差」，不數每個點的轉角（切角越細點越多，會假裝變彎）。
