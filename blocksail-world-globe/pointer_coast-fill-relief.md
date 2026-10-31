---
id: pointer_coast-fill-relief
topic: blocksail-world-globe
title: 補陸地（coast fill: 1）＋局部等高線表：在哪、怎麼畫、坑
type: pointer
status: active
created_at: 2026-10-31
created_by: trailhead
links: []
related_docs: []
---

地圖少畫的陸地補得回來（TASK-1082 聖胡安，2026-10-30、31）：
- 格式：docs/WORLD_FORMAT.md §22 —— tools/bathy_edits.json 的 coast 那一筆寫 fill: 1 ＝ 圈裡併成陸地（world/world.js ringUnion／applyFills 載入時改環）；§22.2 局部等高線表 tools/relief_edits.json（改 tools/relief_rings.py 的 ZONES 再跑，從 world_extract 那份 tiles10 抽低於 50 m 的圈）。
- 範例：tools/san_juan_shore.py（照 OSM 的岸，挖、補一起出）。
- 坑：補的圈往地圖陸地裡多伸 60 m、挖的圈離 OSM 陸地 3 m —— 兩條岸共用邊 ⇒ 判不出交點、留零寬陸橋；relief 不能放進 bathy_edits.json（*_shore.py 改寫表時只寫 _、lines、coast，多一節會被悄悄丟掉）；只補不給高度 ⇒ 平地 0.2 m，生成器主要道路要 h > 0.3 會找不到。
- 檢查：tools/bathy_check.cjs ③④、tools/cut_check.cjs ③。summit 做大西洋四島（特內里費少 48 km²、加拉奇科在水裡）要用這一套。
