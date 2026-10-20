---
id: knowhow_oar-test-ship
topic: constitution-broadside
title: 槳的量測：測試槳船怎麼做、示範模式不吃鍵盤、划槳方向用數字驗
type: knowhow
status: active
created_at: 2026-10-18
created_by: ame
links: []
related_docs: []
---

槳（TASK-0691，Blockside 2d690a3／dev 9ade3cd）量測與接手要知道的：

- 還沒有真的槳帆船 ⇒ 測試船用 scratchpad 的 mkoar.py 做：夜隼號 .vxk 檔頭插 N 支槳模型（方柱網格、meta kind=oar、固定點 pivot／blade）＋ game.json 一筆 oartest（cfg.base 7），在**複製出去的資料夾**建、⛔ 不進 repo（ame 那次在 scratchpad/oartest）。
- `?demo=…&sail=manual` 示範模式不吃鍵盤（dispatch keydown 'a' 舵角照樣 0）⇒ 量迴轉要在測試那份加 `__game.rudHold(v)`（KEYS.l／r），⛔ 不提交。
- 窗格藏著畫面不更新 ⇒ 划槳方向用數字驗：m.swM 套在 (blade − pivot) 上，出力那半拍（D.oph 0…0.5）槳葉 x 要從 +A 走到 −A（往船尾）、z 不動；回槳 z 抬起。⚠ 2026-10-18 還沒有人在畫面上看過。
- 職位編號：帆匠 SAILMAKER 9（trailhead）、槳手 OAR 10；ROLE_NAME 要跟著加（少一格 ⇒ 廣播印 undefined）。
- 驗「沒有槳的船不變」：8 組對拍＋virginia,napoleon＋gloire,virginia drvcheck=1；差動轉向只對兩舷都有輸出的船作用，現有的船都沒有。
