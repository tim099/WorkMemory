---
id: pointer_leakcheck-static
topic: blockside-flood-rooms
title: 漏水檢查（超載極限）是靜態自檢，不是模擬；砲門、門、舵在就不漏
type: pointer
status: active
created_at: 2026-10-27
created_by: unknown
links: []
related_docs: []
---

js/75_leakcheck.js（2026-10-27，Blockside 5f77252／b4dabcc；summit fb266c7 加舵）是**靜態的造船自檢**，進水模擬一行都不讀它：
- 問的是「船本來的樣子，水面升到哪會流進艙裡」——再多載幾噸、吃水再深幾 m、從哪個口。觀察面板「💧 超載極限」、shipcheck 報告一行、`__game.leak(heel, i, hole)`（hole ＝ 記憶體裡挖洞的反向對照）。
- Tim 拍板的規矩：砲門、槳門、門、舵（舵桿／舵柄穿船殼那一段）**在就不漏水**；模擬維持「只算打出來的破口、被打掉的門」（房間模擬 33_wrsim 也明寫船殼的洞不算）——縫是造船要在作品裡堵的，不是改模擬。⚠ TASK-0822（沒蓋板的砲口泡水會進水）跟這條規矩方向不同，合之前要再問 Tim。
- 坑：艙裡不能照全部格子的外框認（船首喙底下、船底板下面、螺旋槳開口全變成艙）；要照 flSkin、兩舷兩頭都真的有船、頭上有蓋腳下有底、≥1.5 m³。flSkin 在砲門那幾列會縮到中線 ⇒ 每列取前後 16 格最外。漏點是「水跨進外框那一格」，不一定是最高那一關（分類照水路最高點更準，summit 改了）。
- 全船隊第一輪的真縫開成 TASK-1037～1043（各船主），定遠號留言在 0779。
