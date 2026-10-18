---
id: pitfall_floor-per-column-placement
topic: sculpt-frigate
title: 甲板上擺東西：地板逐欄量、別用舷弧內插；門前留路
type: pitfall
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

在甲板上找擺放位置（絞盤、裝飾），地板高度要**逐欄直接量**（那一欄由上往下第一個實心 +1），⛔ 不要用甲板 meta 的舷弧 floor 內插。
- 血證（TASK-0685 絞盤）：風暴角號上甲板 x 250 起甲板板低一格，內插判成「腳下不是甲板」⇒ 那一段整段被誤判放不下、只剩 x 242 ⇒ 絞盤堵在後甲板前緣艙壁唯一的門前 ⇒ 遊戲匯入檢查多 ⚠ 火藥 ×4（上甲板 P1、S1、AP、AS 走不到）。
- 判斷「擋不擋路」要看遊戲的走路格會避開每個模型外框（40_crew.js），⛔ 只看格子；有門的艙壁前面留至少 1 m。
- 另一條：放完模型跑一次 Blockside admin.py shipcheck，比「⚠ 火藥」那一行跟放之前逐字相同，才算沒擋到送藥手。
