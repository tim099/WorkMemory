---
id: pitfall_hullgrid-boundary
topic: sculpt-nightlamp
title: HullGrid 邊界內插、只量頂點、迴圈的 \r
type: pitfall
status: active
created_at: 2026-10-14
created_by: basecamp
links: []
related_docs: [Sculpture/works/basecamp-nightlamp-01m/design/draft_floors.py, Sculpture/works/basecamp-nightlamp-01m/design/draft_frames.py]
---

夜燈號 design/ 產生程式踩到、會咬之後每一支（外板、內板、甲板）的坑：

- **HullGrid 夾著 −1 內插**：船殼表船殼外填 −1；沿 x 內插時一欄還在船殼裡（4.07）、隔壁一欄在外面（−1），平均出 0.043 的假半寬 ⇒ 中線舷牆頂浮一排格。修在 draft_floors.HullGrid.column（兩欄有一欄在外面就算外面）；代價：邊界往裡削最多 0.05 m（0.64 格）。第一次猜成「沿 z 內插」改錯一版，格數一格沒變 —— 印出那一格的中間值才找到。
- **只量剖面頂點會漏掉跳的那一段**：船尾 tuck 那裡半寬一下跳 2 m（counter 底面是平的），「離船殼線多遠」只量頂點 ⇒ counter 底面不算船殼線、肋骨側邊浮起來。draft_frames.frame_cells(segments=True) 量到線段（只在兩頭開；階段五量頂點、已雕不改）。同時要擋「這一欄最低有值的高度」，不然內插會在 counter 底下夾出不存在的格。
- **落子迴圈比對 placed 字串要先去 `\r`**：Windows 上 python 寫的清單有 CRLF，`read n c` 讀到 `1964\r`，對的那一刀被判成不對而停下（停對了，理由是假的）。
- 外板（階段七）要擋「從 S 往上長」：不然舷牆頂整片長出蓋子。
