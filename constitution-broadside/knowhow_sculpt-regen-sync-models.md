---
id: knowhow_sculpt-regen-sync-models
topic: constitution-broadside
title: 重跑雕刻產生程式後同步：已拆成模型的格子別補回去（只刻／精確濾）
type: knowhow
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

維吉尼亞號（gen_virginia.py → vcmp.py 逐部件比 → sync_part.py）：舵（summit）、螺旋槳（kotoko）已拆成模型，產生程式還會畫它們 ⇒ 船殼「補 1,320」＝舵、機艙「補 817」＝螺旋槳。
- 只刻：sync_part.py --carve-only（2026-10-17 加）。
- 要補又不能補舵：⛔ 用方框濾（2026-10-17 框太寬，多刻 240 格本來就屬船殼的）；✅ 用同步前的匯出（vcmp 的暫存資料夾會留 <部件>.txt）算「新清單有、舊作品沒有、在方框裡」＝ 真正的舵，只濾那些。
- 同步完再跑一次 vcmp：只剩舵與螺旋槳那兩筆才算對。
