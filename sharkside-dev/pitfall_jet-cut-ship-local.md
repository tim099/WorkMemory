---
id: pitfall_jet-cut-ship-local
topic: sharkside-dev
title: 高壓水柱：落點記船座標、斷不斷照龍骨層連通
type: pitfall
status: active
created_at: 2026-10-31
created_by: unknown
links: []
related_docs: []
---

高壓水柱（Sharkside 04c40dd、核心 gura/idle b562e8d 的 cut）踩過的兩個坑：

1. 刀的落點記世界座標 ⇒ 船一直往前開，上一點在船身上往後滑，一刀拉成斜的一大片（船體報告量：最慘那一截只少 27%、斬不斷）。
   ⇒ 打到的那一點、噴口都記在船的格座標（sh.inv 換進去、sh.M 換回來，一欄一軸），每一格再換回世界座標交給 cut。
2. 斷了沒，第一版照「某個 x 整個斷面（所有 y、z ≤ 12）一格不剩」⇒ 鏡頭沒正對船側時那一片是斜的，每個 x 只切掉一部分，永遠判不斷。
   ⇒ 改成連通：sh.cutK 累積切過的 x 範圍，兩頭外推 2 格，在龍骨層裡從船尾那頭灌、灌不到船頭那頭 ⇒ 斷；錨只留長的那半截（長短照 T.wl0／wl1，⛔ 照格子的框會留錯半截）。

驗法：55_hullrep.js 的船體報告（BSI.hullReport()：側視、俯視、龍骨帶、橫剖面跟 T.base 比）—— 背景窗格的截圖靠不住時，讀它的剖面格數；拖曳用 #cv 上的合成 PointerEvent（固定 pointerId）。
