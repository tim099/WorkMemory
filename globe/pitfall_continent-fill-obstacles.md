---
id: pitfall_continent-fill-obstacles
topic: globe
title: 大陸級只塗空格：障礙只扣別人的格子、逐格對帳
type: pitfall
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

大陸級「只塗空格」的做法（2026-10-09 erina 畫亞洲／非洲／歐洲）：
- 陸地＝Natural Earth 10m land（D:/Unity/Senate/Temp/ne_10m_land.geojson，meadow 下載）的最大多邊形 ∩ 洲界多邊形，簡化 0.05°，切成不帶洞的塊（有洞就四分遞迴）。
- 障礙＝已畫格子：讀 Globe/_cache 分塊（rgb≠0），柵格化 0.01° 並依緯度膨脹約一格（cv2）⇒ 接邊縫 1–2 格。
- 🩸 障礙**只取非我事件的格子**：歐洲第一輪把自己畫的亞洲也當障礙，烏拉爾整條留縫；補線兩次 changed=0 ＝縫不在以為的位置。治本：重放非 erina 事件建障礙、在界線帶重塗。
- 🩸 洲界多邊形要量「外面那一側」：紅海出口那條線切過非洲之角被塗進亞洲；擦之前用「只涵蓋界線外 1°」的清單說沒有別人的筆 —— 尺量不到那裡。改用備份快取逐格比對（painted before 是否改變）才算數。
- 每輪：備份 _cache/tiles_4096 → 跑 → 逐格比對原本畫過的格子 changed=0。
