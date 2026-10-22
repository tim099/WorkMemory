---
id: pitfall_sea-level-zero-assumptions-v2
topic: constitution-broadside
title: 寫死海面＝0 的地方：水面一凹下去就靜默出錯（含雨）
type: pitfall
status: active
created_at: 2026-10-22
created_by: kiara
links: [constitution-broadside/pitfall_sea-level-zero-assumptions]
related_docs: []
---

Blockside 裡寫死「海面 ＝ 0」或「浪高上下限只有浪」的地方 —— 只要水面能凹下去（大漩渦），每一處都會靜默出錯，而且出錯的樣子都很像別的 bug：

- 33_flood.js「比最低的浪谷還低 ⇒ 一定在水下」用 −seaBound ⇒ 漏斗裡甲板上的人被判溺水。修：08_sea.js seaLow（再扣漏斗深度）。
- 01_gl.js 暴風雨白沫照 seaH 判浪頭 ⇒ 漏斗裡整片沒白沫。修：扣掉 vxH 再判。
- 01_gl.js P.abyss（破洞底下的海）照 y 0 的平面 ⇒ 漏斗裡從破洞往下看是霧色。修：uAbY ＝ 鏡頭看的那一點的 vxH。
- 21_gunnery.js／22_pieces.js 碎片「沉到 −25／−40／−60 就拿掉」⇒ 深漏斗裡一出現就拿掉。修：照那一點的水面算深度。
- 76_anchor.js 鐵鍊：海底 −20、在水裡 ＝ y < 0 ⇒ 錨鏈掛到天上（Tim 截圖）。修：照那一點的水面。
- stepWaves 船底十點平均：眼的喉部比船窄 ⇒ 沉船懸在喉底上面 28 m、半分鐘才消失。修：開始沉之後水面換成船中心那一點的。
- 14_rain.js 3D 雨滴的圓柱寫死「−1 m 到 max(鏡頭, 8) ＋ 28 m」、低於 8 m 才查落海（2026-10-22，Tim：「大漩渦內(海平面下的漩渦表面) 雨會消失」）⇒ 鏡頭在 −200 m，雨下在頭頂兩百公尺的霧裡、落海 0 次。
  修：底／門檻／頂照圓柱內側、外側、中心的漏斗水面；查落海前先拿 vxH ＋ seaBound 篩（不篩 ⇒ 每滴每格算整片浪，3.0 ms 對 0.8 ms）。
  ⚠ 畫面那一層的雨絲（01_gl.js P.rain，中畫質）沒看高度，所以「中畫質有雨、高畫質沒有」—— 同一隻 bug 只在一種畫質現形。
- 還沒修：同一個原因（漏斗是彎的、船身是直的），深 250 m 時船身兩端泡水 ⇒ 落水爆量。修法：船的泡水判定用船自己擬合的平面。Tim 說先不修。
⇒ 下次要讓水面變形（海嘯、潮汐、河流），先 grep 這幾處跟 `seaBound`、`< 0`、`< 8`、`-ANC_DEPTH` 一類的常數；只給人看的特效（雨、水花、霧）也算。
