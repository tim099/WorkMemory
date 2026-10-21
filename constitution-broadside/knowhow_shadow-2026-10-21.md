---
id: knowhow_shadow-2026-10-21
topic: constitution-broadside
title: 陰影現況：單張預設、分段貼合畫面、畫質頁細項、樹的邊界
type: knowhow
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

陰影（60_render.js shadowPass）2026-10-21 的樣子與坑：

- 預設單張（shadowSimple）：一張跟著鏡頭、每一格整張重畫；半寬分六級、換級有重疊。Tim 10-20 確認不閃。
- 分段（cascade）照畫質頁「陰影細項」調（07_gfx.js SHK_ITEMS；照 Unity 的 Shadows：最遠距離、段數、分界 1–3、交界 1–4、顯示分段）。分段的陸地快取預設關 ——
  10-20 那個「一大片暗一下」就是快取（靜態那一層）跟主畫面差一格；地形網格在畫陰影之後才建。要開快取先想清楚這一點。
- 分段貼合畫面（csmFit，CPU 版 SDSM）：畫面 8×6 條線沿地形（bedHgt）走，取最近有東西（海上 80 m 桅頂、陸上 40 m）到最遠地面那一段，分界照比例縮進去。
  以前從鏡頭量 ⇒ 鏡頭拉高時第 1、2 段整段在空中（Tim 的截圖：紅黃框不罩船）。參考 dihara/cascaded-shadow-maps（MIT）；GPU 讀回深度圖會卡一格，所以在 CPU 做。
- 陰影圖：中 2048、高 4096，總量上限兩張 4096²（分段 3、4 段 ⇒ 一層 2048）；全開 ＝ 高 × 距離 1.5（shFar）。
- 樹：立體樹的距離是畫質項（400／650／900／1350 m），樹收影子到「那個距離 − 50 m」；外面是貼圖與樹冠殼，不參與陰影 —— 「開關分段陰影範圍沒變」就是這條邊。
- 驗閃一定要在真的在跑的幀看（Tim 的截圖＞同步 render() 的合成測試）。
