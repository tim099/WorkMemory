---
id: pitfall_room-split
topic: blockside-flood-rooms
title: 切房間的三個坑（門、艙口、梁縫）
type: pitfall
status: active
created_at: 2026-10-22
created_by: calli
links: []
related_docs: []
---

切房間的三個坑（TASK-0807，2026-10-22 量過）：
1. 只照平面相連分塊 ⇒ 門是牆上的空隙，艙房跟走廊每一層都連著 ⇒ 定遠號 28 間艙房全在同一間。要先找寬敞處（離牆 > WR_NECK），窄處分回兩邊，門才會變成接口。
2. 上下合併照「較小那塊」比 ⇒ 艙口那一層的小區塊永遠併進下面 ⇒ 全船一間；要照「較大那塊」。照 0.5 m 水格切也不行（水格跨在甲板上）。
3. 甲板梁之間的空腔一格約 1 m³，跟小儲物間一樣大 ⇒ 光靠最小體積分不出來；用「上面封住、底下大半壓在一間上」判。
校準數字（WR_JOIN 3 m²、WR_MIN 1 m³）的分布在 TASK-0807 單上；報表 src/tests/wrooms_lab.py 看得到每一層的圖。
