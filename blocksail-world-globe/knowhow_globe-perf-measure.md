---
id: knowhow_globe-perf-measure
topic: blocksail-world-globe
title: 地球儀卡頓先量：卡的是整張重畫的點陣化，不是載入
type: knowhow
status: active
created_at: 2026-10-21
created_by: kiara
links: []
related_docs: [D:/Unity/Blocksail/docs/WORLD_FORMAT.md]
---

Blocksail 地球儀（globe.html）的效能：先量，別照猜的修。2026-10-20 Tim 懷疑「航行時要一邊載入周圍細節格」；量下來不是：跟著船時 loadTick 一直被重設、根本不載，卡的是每一幀整張地圖重畫（Canvas2D 幾千圈 Path2D 填色的點陣化；JS 約 20 ms，其餘在 GPU）。

- 量法：內建瀏覽器隱藏時 rAF 不跑、計時器被節流 ⇒ 數字是假的；headless Chrome 是軟體點陣 ⇒ 也不準。要開真的 Chrome（--window-position=0,0、--disable-features=CalculateNativeWinOcclusion、--force-device-scale-factor 1／2）走 CDP：rAF 間隔＋long-animation-frame（scripts ≥ 5 ms 帶函式名）＋Profiler。
- 讀數（RTX 4080 筆電、1906×986、比例 60、跟著船）：1 倍 30 ⇒ 60 fps、2 倍 8～10 ⇒ 60 fps；出發那一下 A* 1.1 秒 ⇒ 搬進 world/route_worker.js。
- 修法：moveDraw 底圖（四周留邊、三點仿射貼上、超前船頭、誤差 ≤ ½ 像素才續用：位移 × 角落距離² ÷ 2R²）。驗法：貼圖那一幀對整張重畫逐像素比（blit_check）。
- 還頓的：重畫底圖那一幀（2 倍、1 秒＝1 小時約 3.4 秒一次 130～160 ms；越拉遠越常）—— 要再省得分幀畫或改 WebGL。

細節與讀數在 Blocksail docs/WORLD_FORMAT.md §17「航行時的畫面」。
