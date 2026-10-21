---
id: decision_materials-weight
topic: sculpt-dingyuan
title: 外板、砲塔頂、海圖室的混合材質與重量帳
type: decision
status: active
created_at: 2026-10-22
created_by: unknown
links: []
related_docs: []
---

材質與重量（Tim 2026-10-21 拍板）：
- 外板 iron_hull ⇒ 遊戲 matMap iron*0.5+oak*0.5（pen 115：混鐵就走鐵甲穿／凹／彈）；砲塔頂、海圖室 iron_plate ⇒ matMap iron*0.125（2 格裡一吋鋼板）。雕刻那側 Sculpture/materials.json 有 iron_hull、iron_plate 兩列（觀測頁反光），遊戲材質表沒有這兩個 ID，靠定遠號自己的 matMap。
- 壓艙物是自動的（T.ballast0 ＝ 設計水線排水 − 格子 − 模型，25 噸一格）：船太重時是 0、扣不了 ⇒ 只能從格子的材質減。量重量看 __game.ships()[0].mass0／T.disp／lift，瀏覽器存檔可能留著舊的壓艙物。
- 現況 7,292 t（設計水線 7,288）。
