---
id: knowhow_deck-surfaces-folk-stairs
topic: blocksail-town-roads
title: 碼頭、棧橋、樓梯是地形上方的板子：鎮民照面板走、skala 上岸路補根（2026-10-26）
type: knowhow
status: active
created_at: 2026-10-26
created_by: unknown
links: []
related_docs: []
---

碼頭線（海堤）、棧橋、樓梯是畫在地形上方的板子（Blockside 63_towns.js townGroundTile 的 slab、台階），照地形取高度的東西都會陷進去。2026-10-26：
- 鎮民（63_townfolk.js）：碼頭、棧橋的中心線也進路網；每條邊建路網時記下碰得到的面板段、樓梯（e.dk／e.sk，16 m 格子查）⇒ 畫的時候只量那幾塊（townFolkDeckY）。讀數 __game.town().towns[i].folk.deck。君堡建路網 48～57 ⇒ 74.5 ms（全掃 144 ms）。
- 樓梯的階高照畫台階同一套（路那一頭照地形重量、階數重算；fix0 照規劃）—— 另寫一份會跟畫面差一階。
- 生成的 skala 上岸路以前從根往陸上 4 m 的門口才起 ⇒ roadStairs 射不到（STAIR_REACH 2 m）、也不報 issue ⇒ 面差 0.9～1.2 m 直接斷開。town.js 已補根那一點；手擺的 skala（君堡 skala-venice、skala-plateia，加拉塔 pera-scala-west）照舊沒有樓梯。
- 量「以前陷多深」只算陸上（地形 > 0.05）的點，水上的點以前本來就不畫。
