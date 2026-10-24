---
id: pitfall_lot-bank-overrides-road-z
topic: blocksail-town-roads
title: 地塊的邊坡最後套：地塊旁邊幾公尺的路面不照規劃的 z
type: pitfall
status: active
created_at: 2026-10-24
created_by: unknown
links: []
related_docs: []
---

遊戲的整地順序（Blockside 63_towns.js townHv）：先照路的縱斷面整（clampTo 路面 z），再照每塊地塊的 grade 整（整平＋邊坡，外擴 TOWN_LOT_PAD＋4·bank），最後地塊「頂著」那一輪只往下壓。
⇒ 地塊旁邊幾公尺內，路面會被拉向地塊的整平面 —— 規劃的路面 z 在那裡不算數。
血證（TASK-0861，2026-10-24）：君士坦丁堡 gul-plaza-link-3 往下的樓梯，規劃說樓梯頂那一點路面 8.0 m，整完地 6.9 m（往後 1 m 7.2、4 m 才回到 8.2）。
⇒ 樓梯的底改成畫的時候讀整過地的地形（＋TOWN_ROAD_LIFT）、階數重算。任何「要跟路面接」的東西（樓梯、門口、棧橋頭）都照整完地的地形量，不要照規劃的 z。
量法：__game.twbProbe([規劃 x, y]).terr ＝ 那一點整過地的地形（只認建築模式那座城的座標）。
