---
id: pitfall_stage6-precarve-verifier
topic: sculpt-frigate
title: 階段六雕之前 verifier 抓到的坑（讀數照抄計畫、HullV2 nan、中線縫）
type: pitfall
status: superseded
created_at: 2026-10-11
created_by: unknown
links: [sculpt-frigate/pitfall_stage6-carve-lessons]
related_docs: []
---

階段六（船頭與船尾）雕之前派 4 個 verifier，抓到的坑（2026-10-11；都還沒修，清單在作品 todo「雕之前修」）：
- **讀數照抄計畫**：taffrail 計畫 Z 125–128，清單只有 125 一層；verify_frames_ends.json 印的 z 是 plan 的參數，不是清單的 bbox ⇒ 沒有一行在叫。⇒ verify 一律從產物（清單、events）量，計畫值只拿來比。
- **HullV2 的 nan**：`draft_ends._taper` 在 8 號站頂（126.19）以上量不到 9 號站斜率（只剩一站 ⇒ 樣條 nan），整排船尾水線變 nan ⇒ 船尾上部的船殼只到 Z 126。修：nan ⇒ 0。
- **船頭的 hb 在 z_top 以上不會變 nan**（表的 z 索引被夾住）⇒ 判斷船殼內外要另外檢查 z ≤ top。
- **dy ≥ 3 不是到處都對**：留給龍骨／艉柱的那 5 格，在艉柱頭以上沒有東西 ⇒ 船尾橫材在中線開了一道 0.5 m 的縫。只在有柱子的高度留。
- 在平面裡量「離船殼多遠」時，舷頂的切口要排除（往上延伸船殼再量），否則每根木料頂上多一條水平帶子（第一版 knighthead 頭上長成 35 格長的方塊）。
- 「垂直於船殼的距離」對 X 方向的偏移是鈍的（偏 3 只報 0.7–1.9）⇒ 船頭這種陡的地方要報水平距離與收尾點差。
- 斜的木料用 `sculpture op=stampvox`（TASK-0487，3D 格子清單、一刀一根）；清單在 `design/masks/ends/*.vox.txt`，at=0,0,0、expect_pixels＝清單格數；舊 exe 讀不懂 stampvox 事件。
