---
id: knowhow_maelstrom-1022-helm-takeover
topic: constitution-broadside
title: 大漩渦 10-22：接管航行表、電腦的邊緣、眼往下拖、漏斗裡的雨
type: knowhow
status: active
created_at: 2026-10-22
created_by: kiara
links: [constitution-broadside/knowhow_maelstrom-architecture]
related_docs: []
---

大漩渦 2026-10-22 三輪（TASK-0683 已結，dev 5670613）之後，改它之前先讀這段（接在 knowhow_maelstrom-architecture 後面）：

- 接管航行：54_helm.js `HELM_TAKES`（k／name／pri／fn／選填 bound）＋ `helmTakeover`，31_sailing.js 只叫它一次。順序：避開淺水 10 ＞ 逃離漩渦 15 ＞ 讓友船 20 ＞ 讓敵艦 30 ＞ 讓砲線 50；第一個回航向的接管（`sh.helmMode`，詳細資訊「自動操舵・…」）；bound 是別人接管時也套的限制（vxBound：逃的時候不准往中心）。新的接管情況 ＝ 加一列，⛔ 別再往 31_sailing.js 串 if。沒有漩渦時順序變了也逐位元組不變（漩渦那列回 null）。
- 逃離漩渦排在讓船前面的理由：排後面時讓友船／讓敵艦的航向剛好朝中心，船被「讓」進 45 m；只加 bound（壓成切線）也不夠，47 m 處切線抵不過往內 3 m/s。
- 邊緣：`VX.aiR`（天氣設定頁「電腦避開半徑」80 m）與 `vxShipEdge`（往內水流 `vxInV` ≤ 0.8 × `sh.vxCap`）取大；`sh.vxCap`（31_sailing.js）只看帆與主機的完好度 ×√½，⛔ 照當下航向／收帆算 ⇒ 邊緣在 80–400 m 亂跳。逃離航向 `vxEscYaw` 避開頂風（固定方向時剛好頂風，船被倒拖進眼）。
- 眼往下拖：`vxDrag` 照 `vxPullV`（pull 0.4 ×（1 ＋ 深度／2））；眼那一份 `vxOffset`（深度 ＋ 半長×|sin 縱傾|，支點在翹起那一頭）每步照變化量加在吃水上；粒子模式的進水那一份另外慢慢跟（`sh.sinkF`）。判沉沒 `vxSunkAt`。2.5 秒開始沉、8 秒沉沒。
- 14_rain.js 的 3D 雨滴圓柱照漏斗水面（見 pitfall_sea-level-zero-assumptions-v2）。
- 量法：電腦對電腦的戰局統計用 `?headless=1`（不然畫面迴圈會跟 __game.run 交錯，同種子跑出不同局）；同一頁連跑兩次 fleet() 不是同一局，要重新載入；run() 的分段大小也會改戰局。進眼比例只量了兩個種子（seed 3：dev 3/4 → 2/4），被打殘、沒人操船的船漂進去不算錯（Tim：「大約60%不被吸入即可」）。
