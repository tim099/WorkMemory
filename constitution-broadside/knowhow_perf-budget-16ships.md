---
id: knowhow_perf-budget-16ships
topic: constitution-broadside
title: 16 艘時每格畫面的成本分布、量具（rprof／frames）、三個減負的開關
type: knowhow
status: active
created_at: 2026-10-14
created_by: ame
links: [pitfall_stale-vxk-cache]
related_docs: []
---

2026-10-14（wake #17，e68f7c7）量的：16 艘 8 對 8（憲法號／餘燼號／夜隼號混編）、中畫質、全開物理。
- 每格（__game.frames({sum:true})）：帆繩物理 67.6／畫面 45.9／碎片模擬 24.4（36,000 顆）／砲彈 12／船員 9.6 ⇒ 全部 192.8 ms。
- 畫碎片幾乎不花時間（__game.rprof：拿掉碎片重畫 33 → 37 ms，雜訊內）⇒ 碎片要砍的是模擬，不是畫面。陰影關 ⇒ 約 −9 ms。
- 減負（都只動畫面，#dump 逐字相同）：碎片上限 DEB_CAP 20,000（24.4 → 6.1 ms）；遠方帆繩 rigfar（46.5 → 28.3／21.1 ms）；物理減速 PSLOW（不省時間，換成慢動作）。
- 量具：`__game.rprof(k)`（同一格重畫 k 次、gl.finish）、`__game.frames({clear})` ＋ `live(sec)`（真的畫面迴圈）、`__game.pslow()`、`__game.ships()[i].rigM`。
- 坑：預覽窗格在背景時 live() 的 render 會量到 0.7–1.5 秒一格（GPU 降速），rprof 同時只有 30 ms ⇒ 畫面的絕對值要在前景或真的瀏覽器量。
- 坑：fleet() 之後馬上 render（__game.gpu）會因為 CAM.vp 還沒算掛掉 ⇒ 先 live(0.1) 走一格。
- 上限：limits.maxShips 從 3abb5fb 起就是 16（不是 8）——講數字前先讀 config。
