---
id: decision_district-links-hand-drawn
topic: blocksail-town-roads
title: 城區之間的路手拉、只自動檢查連通（links 欄、town_link.js、town_bake 連通關）
type: decision
status: active
created_at: 2026-10-27
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-27 拍板：城區之間的路是**手拉的**，工具只檢查連通（取代 TASK-0988 原本的「每個遠城區自動接回主城區」）。連法只要整座城的城區連得通就好（A ⇒ B ⇒ C 也算），不必每區都直接接主城區。

- 資料：範圍層那座城的 `links: [{id, path:[[lon,lat]…], note?}]`，⚠ 放在 `districts` 前面（town_districts.cjs 要 districts 最後）。id 不可跟城區撞，城區 id 也不可用 `link-` 開頭。
- 規劃單位：每條路是一份只有路的規劃 `<城>~link-<id>`（world/town_link.js 的 townLinkPlan）；坡度、填挖照主幹道 PROF，兩頭釘在碰到的那條路的高度。
- 草擬：`tools/town_link_draft.cjs --from <城區> --to <城區> --id <id> [--write 1]`（16 m 格 A*，> 18° 或水擋掉，80 m 拉直；24 m 拉直會出現格狀折角）。草擬只是第一版，手改 path 才是正本。
- 檢查：`tools/town_bake.cjs` 最後一關每座城印每條路的 km、坡、填、埋、接到哪；✓「城區都連通」或 ✗ 列出哪一區沒接上、哪一頭懸空。
- 載入：townsNear 會把路的單位一起列（reach＝半條對角線＋500 m），townsForArea 用 townLinkOne 建。
- 沒驗：地球儀「畫範圍」存檔會不會把 links 洗掉。
