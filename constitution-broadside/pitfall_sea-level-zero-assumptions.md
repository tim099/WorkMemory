---
id: pitfall_sea-level-zero-assumptions
topic: constitution-broadside
title: 寫死海面＝0 的地方：水面一凹下去就靜默出錯
type: pitfall
status: superseded
created_at: 2026-10-18
created_by: kiara
links: [constitution-broadside/pitfall_sea-level-zero-assumptions-v2]
related_docs: []
---

Blockside 裡寫死「海面 ＝ 0」或「浪高上下限只有浪」的地方 —— 只要水面能凹下去（大漩渦），每一處都會靜默出錯，而且出錯的樣子都很像別的 bug：

- 33_flood.js「比最低的浪谷還低 ⇒ 一定在水下」用 −seaBound ⇒ 漏斗裡甲板上的人被判溺水。修：08_sea.js seaLow（再扣漏斗深度）。
- 01_gl.js 暴風雨白沫照 seaH 判浪頭 ⇒ 漏斗裡整片沒白沫。修：扣掉 vxH 再判。
- 01_gl.js P.abyss（破洞底下的海）照 y 0 的平面 ⇒ 漏斗裡從破洞往下看是霧色。修：uAbY ＝ 鏡頭看的那一點的 vxH。
- 21_gunnery.js／22_pieces.js 碎片「沉到 −25／−40／−60 就拿掉」⇒ 深漏斗裡一出現就拿掉。修：照那一點的水面算深度。
- 76_anchor.js 鐵鍊：海底 −20、在水裡 ＝ y < 0 ⇒ 錨鏈掛到天上（Tim 截圖）。修：照那一點的水面。
- stepWaves 船底十點平均：眼的喉部比船窄 ⇒ 沉船懸在喉底上面 28 m、半分鐘才消失。修：開始沉之後水面換成船中心那一點的。
- 還沒修：同一個原因（漏斗是彎的、船身是直的），深 250 m 時船身兩端泡水 ⇒ 落水爆量。修法：船的泡水判定用船自己擬合的平面。Tim 說先不修。
⇒ 下次要讓水面變形（海嘯、潮汐、河流），先 grep 這幾處跟 `seaBound`、`< 0`、`-ANC_DEPTH` 一類的常數。
