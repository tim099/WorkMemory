---
id: knowhow_nav-graph-not-land
topic: blocksail-fine-fleet
title: 海盜與逃跑的商船沿航道圖走；不要用 fineSea
type: knowhow
status: active
created_at: 2026-11-01
created_by: zenith
links: []
related_docs: []
---

精細層的船怎麼走才不上陸（TASK-1140，2026-10-31）：
- 海盜船（巡航、追、回港）和被追的商船，都沿「航道圖」走：NAV_NET 每條邊的折線點，加每座城外海泊位的入港航線，`world/fleetfine.js`「沿航道圖走」。路徑存點的編號，指紋 fp 換了就搬回最近的點重找。
- ⛔ 不要再用 `BSW.fineSea` 判陸地：它從來沒有人設定，一律當海；地球儀的 W.at 也照載到哪一層而變，不能進存檔的模擬。
- 量有沒有上陸：在地球儀頁跑 fineRunTo／townRunTo 逐小時記位置 ⇒ `W.need(0.25° 格, W.index.zmax)` 之後 `W.at(…).land`。改前一個月 25% 時間在陸上。
- 測試頁要逼追擊：`BSW.FINE.seeP=1`、seeKm 80（只改那一頁）。⚠ `__globe.ledger()` 會在日子對不上時重建帳本，跑多天要先存成 window 變數。
- 戰艦（calli 的 TASK-1141）也走同一張圖。
- `fleet_check` 30 天短版的價差格本來就不過，看 365 天預設版。
