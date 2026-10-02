---
id: decision_senate-sculpture-decisions
topic: sculpture-senate
title: 雕刻搬 Senate 的拍板
type: decision
status: active
created_at: 2026-10-02
created_by: calli
links: []
related_docs: []
---

雕刻（Sculpture）搬到 Senate 的拍板（Tim 2026-10-02，TASK-0377）：
- 資料層（events／sculpt_cache.json／box／carve／stamp／slice／export／exhibit）在 SCP_Core `Runtime/Sculpture/`，語意與 python 相同（對拍 397/397；81 筆真 events 重放快取逐位元）。python sculpt.py 已刪。
- 渲染只定介面 `ISCP_SculptRenderer`（SCP_Core 不准引用套件、也被 Unity 編）；GPU 實作 `Senate.Desktop/SenateSculptRenderer`（隱藏 GLFW＋FBO），CLI 啟動註冊。Senate.Server 沒有渲染器 ⇒ 出圖大聲失敗、落子照常、分享寫明跳過。
- **引擎出圖與觀測頁同一套**：頁面 spawn 自己的 exe 跑 `cmd sculpture op=view out=…`，⛔ 頁面不自己畫。
- view 輸出：persona ⇒ `letters/<P>/cmd/sculpture_view.png`，或 `out=<絕對路徑>`；⛔ 不再有共用 `_last_view.png`（同 TASK-0374 的病）。
- 渲染設定檔：共用 `Sculpture/render_profiles/` ＋ 個人 `letters/<P>/sculpture/render_profiles/`，疊加 內建 → 共用 → 個人 → 展品 → CLI，只蓋有寫的欄位；不認得的鍵＝錯誤。
- **展品 preset 的舊光照欄位（light_dir／ambient／smooth／shadow）不套用** —— 光照歸設定檔（4/5 件是 auto-exhibit 填的預設值，照 python「preset 優先」會把多光源整組換成一盞白光）。
- 共用預設：星空 rogland（yaw −142／tilt 38）＋量尺網格地板（外擴＝最長邊×0.5、上限 24）＋主光 (−0.4,−1,−1.2) 投影＋暖冷兩盞弱補光；鏡頭＝舊等角（正交 yaw45 pitch30）。Tim 不要微透視。
- 決定性：同機同驅動同輸入同圖；⚠ 換顯卡不保證逐位元。
