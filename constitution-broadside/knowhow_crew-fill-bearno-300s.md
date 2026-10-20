---
id: knowhow_crew-fill-bearno-300s
topic: constitution-broadside
title: 量補位要看戰報 bearNo、打 300 秒（120 秒量不到）
type: knowhow
status: active
created_at: 2026-10-19
created_by: erina
links: []
related_docs: []
---

量「砲有沒有人」的改動（補位、候補、船員調度）要看戰報 bearNo，不要看開火數。

- 戰報（?report=1）每門砲有 bear（裝好、目標在射界內的秒數）與 bearNo（目標在射界內、砲卻沒人的秒數；TASK-0722 加的）⇒ 比例 bearNo ÷（bear＋bearNo）。
- 開火數、陣亡被軌跡主宰：任何改動都會讓對局分岔（同一種子 ±20%，還會撞船），六場也分不出來。
- 要量到補位的效果得打久：120 秒那組量不到（預備水手還沒用完；關 8.9%、開 8.7%／11.1%），300 秒才拉開（22.2% → 10.0%，3 對 3 每個種子都改善）。
- 量法：gf_full.py 帶 report=1&headless=1、秒數 300；單挑憲法號 vs 餘燼號 seed 3/5/7 ＋ 3 對 3（blue constitution,ember,falcon／red constitution,tridente,nightlamp、cmd line,line）seed 11/12/13；開／關各建一份成品（idle.r 2／0），白天一次只跑一個。
- 人在兩舷之間來回搬會把好處吃掉（三叉戟號 120 秒 105 人次）⇒ 調過一次的不再調、剛收回來的組 30 秒內不調。
