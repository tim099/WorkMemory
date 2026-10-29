---
id: knowhow_new-island-town-recipe
topic: town-replica-from-plans
title: 新蓋一座小安地列斯的城：量岸差、修岸、拉港口等深線、挑種子、城區、開場檔、航線快取
type: knowhow
status: active
created_at: 2026-10-30
created_by: unknown
links: []
related_docs: []
---

新蓋一座小安地列斯的城（TASK-1086 布里奇敦，照 trailhead TASK-1084 巴斯特爾的提交訊息走）：
1. 先量岸差：harbour_grid --at 島中心 --r 24000 --step 50 數陸地格 × 面積，跟島的實際面積比；多了 ⇒ OSM relation（Nominatim 搜 "<島> island" 找 place=island 的 relation ⇒ api/0.6/relation/<id>/full.json，下載要 Tim 點頭）⇒ 接成一圈 ⇒ <島>_shore.py（guadeloupe_shore.py 那一套）寫 coast。
2. 套用一律用 `git show 900c4f1:tools/bathy_extract.py` 那一版（main 那一版表不動也改 carib-52／62／63／71／83／84）；先表不動跑一次，git status world/bathy 要空。
3. 港口：大船泊位不夠多半是岸外一片 1～4 m 淺棚 ⇒ harbour_deep.py（yallahs 那組：basin 300～600、half 160～250、smooth 500、flare 1、ramp 20,45,80、shore 20）。Tim 10-29：海陸資料可能因年代或史料不精確而不同，港口相關的等深線可以手動調讓船停泊、通行。
4. 種子照 town_report 自檢挑，不看 issues 條數（生成器會放自己 berthBuildTry 不收的泊位）。
5. 手寫範圍那一筆結尾停在 range 那一行（帶逗號）⇒ town_dock --write 1 ⇒ town_districts.json 加種子（城內、每條主要道路的中段、路尾的田）⇒ town_districts --write 1；districts 要最後一欄。
6. 開場檔：town_start --write 1 跑兩次 ⇒ 還原被順手改的別座城 ⇒ land_start、forage_start --write 1（巴拉科阿那一筆不碰）⇒ open_list --town <id> --write 1。
7. 航線快取：nav_ports ⇒ nav_net ⇒ nav_bake（NODE_PATH 指 codex runtime 的 node_modules）；nav_stale 三份 ✓。
8. 反向對照：worktree 全部重烘，其他規劃跟 main 烘好的逐一比 plan 內容。
