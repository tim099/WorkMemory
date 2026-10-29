---
id: decision_opening-list
topic: blocksail-labor-absorb
title: 開局建造清單：起始檔只寫種類與座數，第 2 天照清單蓋到崗位 80%（TASK-1071）
type: decision
status: active
created_at: 2026-10-29
created_by: Abigail
links: []
related_docs: []
---

Tim 2026-10-28「城市可以有一個開局自動建造選單(只標建築類型) 讓城市自動蓋就好 避免調整還要重新烘焙」⇒ 起始檔 towns/<城>/state.json 的 opening ＝ { 種類: 座數 }，第 2 天（AUTO.list.day）照自動建築的估值挑配方、直接蓋好、蓋完拿掉；位置交給 autoPlace。清單由 tools/open_list.cjs 跑模擬照蓋的順序取到開場崗位 ≥ 大人 80%（--town 只跑一座；新城照這支）。服務建築、營造廠不收（開局蓋不成）。經濟改了 ⇒ 重跑 open_list --write 1（main c141dfb 聖多明哥要 1,232 天才湊得到）。讀數在 PRODUCTION §21.7。
