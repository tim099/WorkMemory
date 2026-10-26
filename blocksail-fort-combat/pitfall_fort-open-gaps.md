---
id: pitfall_fort-open-gaps
topic: blocksail-fort-combat
title: 堡壘第一版的缺口：砲組不跨半邊、船穿得過、平衡沒調
type: pitfall
status: active
created_at: 2026-10-26
created_by: summit
links: [blocksail-fort-combat/decision_fort-is-fixed-ship]
related_docs: []
---

- 砲組照砲軸 y 的正負分兩半（10_ship.js g.side、40_crew.js reassign 每層每舷各自調度），不跨半邊補人 ⇒ 四面都有砲的堡壘，背對敵人那一半的人閒著。要改就改 reassign 的分組（照「現在對不對得到目標」分），會動到船的戰局，要反向對照。
- 碰撞：collide 直接跳過 fixed 那一對 ⇒ 船開得穿過堡壘。港口戰前要補（照格子判，或把堡壘的格寫進海床／擱淺那一層）。
- 平衡：9 門 6 磅砲的稜堡打不過 44 門巡防艦（seed 7：堡壘挖掉 58,409 格失守、船沒人倒）。駐軍站在露天的胸牆後，被火槍打得很兇（第一場火槍 13 人）。
- 材質 brick 1.15／stone 1.3 是〔佔位〕：只有一個沒寫書名的「砌體 3 呎 vs 木頭 3 呎 10 吋」。
- 量測：每步時間是藏著的分頁量的；有視窗的瀏覽器還沒量。
