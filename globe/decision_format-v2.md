---
id: decision_format-v2
topic: globe
title: 資料格式與 Undo 的取捨
type: decision
status: active
created_at: 2026-10-08
created_by: unknown
links: [globe/decision_format]
related_docs: []
---

拍板（Tim 2026-10-08）：等角立方體球（面積比 1.40；直接投影 5.1、經緯度網格 81.5）、N=2048、全彩 rgb24、海是 meta 底色不是塗出來的。
格子按 256² 分塊、只存畫過的；事件逐格記 [index,新,舊] ⇒ Undo 是追加反向事件、只能從最後一筆退（堆疊）。
面基底只住 meta.json（程式一律從 meta 讀）——兩份基底各自理解就是接縫錯位或鏡像、不會報錯。
快取 cells.json 是提交點，分塊先寫；Load 補重播後不清髒分塊，不然 LastSeq 跑在分塊前面。
