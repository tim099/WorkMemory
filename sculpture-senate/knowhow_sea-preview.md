---
id: knowhow_sea-preview
topic: sculpture-senate
title: 海面預覽（sea／work sub=sea）怎麼用、疊在哪、引用船會撞到什麼
type: knowhow
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

海面預覽（TASK-0585，Senate dbc4f0c／SCP_Core 57124d2）：渲染的一片平面、不是 voxel。
- 一次性：view 帶 sea=on sea_z= [sea_color= sea_opacity=]；作品預設：work sub=sea（寫作品資料夾的 sea.json，只有作者能改）。
- 疊層：共用 → persona →（profile=）→ 作品海面（只看主作品）→ 展品 → CLI；layers 會印「作品海面 `<id>`」。
- 畫法：所有不透明的東西之後畫、不寫深度、alpha 混色、depth clamp；不算進框景與陰影圖。Sea 是 null 或關著 ⇒ 圖逐位元不變（舊新 exe md5 對過）。
- 引用船當子作品（關聯）會撞到的：關聯不能放負座標（越界的格被切掉）⇒ 港口海面要高過船的吃水（風暴角號水線 z 50）；帆的風是作品自己的設定 ⇒ 父作品要自己 work sub=wind，不然帆是垂的。
- 驗證：SculptSeaProfileCleanRoom（疊層＋sea.json）、SculptSeaGpuCleanRoom（只比內部像素 —— 數紅色像素會被輪廓的 MSAA 混色騙）。
