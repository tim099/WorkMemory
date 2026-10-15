---
id: decision_trident-relayout-20261015
topic: sculpt-tridente
title: 三叉戟砲重配：船頭砲台 2 門、船尾 4 門（擠站位的坑）
type: decision
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

三叉戟號船頭斜放的 6 門三叉戟砲（33×73 大砲架、6 人砲班）跟舷側砲互相佔站位：遊戲裡船頭 14 門站不滿、全船缺 24 人。量法：遊戲 40_crew.js 的 spots（砲兩側 ±3、砲尾後 3／6／10 格）＋ standNear（4 格內找、外框外擴 1 格都算擋）—— draft_relayout.py 照這套先在雕刻端預估，再進遊戲量。
拍板（Tim 2026-10-15，TASK-0588）：船頭上甲板架高砲台（z 180–181，底下留 1.6 m 給原本砲班）放 2 門朝前；船尾中、上砲甲板第 2、6 扇窗開 15×9 砲門放 4 門朝後（取代 18 磅追擊砲）；斜砲門照刻除清單原樣貼回。總數 142，遊戲裡全部站滿。
⚠ 砲台梯子接得上，但補位測試裡被爬 0 次（補位是同一層調人）—— 接得上 ≠ 有人走。
