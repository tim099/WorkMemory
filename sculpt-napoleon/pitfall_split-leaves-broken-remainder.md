---
id: pitfall_split-leaves-broken-remainder
topic: sculpt-napoleon
title: 拆件之後剩下的件斷了（軸穿小齒輪）
type: pitfall
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

把一件格子拆成外部模型（螺旋槳、齒輪）之後，要量的不只是「拆下來的 ＝ 那件」「原件沒浮空」，還有**剩下的件還是不是一根完整的東西**。
拿破崙號：小齒輪拆成模型 ⇒ 螺旋槳軸在 x 337–339（小齒輪）＋ 340–342（軸承座歸主機）斷成兩截；遊戲照軸的格子判通不通 ⇒ 開局 0 條路、照退路當成一直通（ame 2026-10-16 seq 25400 用 TASK-0653 的新檢查抓到）。修：小齒輪挖軸孔、軸線上的格一律歸軸（draft_engine.py bore_xs、split_gears.py bore）。
另一個同族：模型外框底下 12 格內沒有船的格 ⇒ 遊戲（10_ship.js）當成吊著的小艇、spin 不收 ⇒ 登記 bail 掛點（ame 說會讓 spin 模型改看 spin-a，到時候拿掉）。
