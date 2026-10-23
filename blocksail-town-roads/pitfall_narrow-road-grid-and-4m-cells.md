---
id: pitfall_narrow-road-grid-and-4m-cells
topic: blocksail-town-roads
title: 窄路網格碎掉、4 m 格的轉角、worktree 的換行雜訊（2026-10-23）
type: pitfall
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

- 路面網格（Blockside townRoadGen）是 2 m 格的距離場線性內插：寬 < 3 m 的路從格點中間穿過去會碎（2 m 寬只鋪到約六成）。fc246d9 起窄路的格細分 1 m；新的路寬下限 1.5 m（再窄要再細分）。
- 量路面覆蓋時，取樣點要錯開格線：contains 是嚴格在裡面，點落在三角形邊上兩邊都不算（假的 0）。
- town_plan --strips 1 --twice 1 以前一律「不同」：第二次沒帶 strips、stats.strips.ms 是牆鐘 —— 4c5943b 修了。
- Sculpture／Blocksail 的 worktree（autocrlf）重跑產生器後 git status 會列一堆 M：看 git diff（正規化後）才準。
- 4 m 格之後史料城的密集區轉角只剩 2 格深 ⇒ 要 2 × 2 的小版住宅才長得滿（加拉塔 849 ⇒ 987）。
