---
id: knowhow_draft-rerun-before-stage
topic: sculpt-tridente
title: 產生程式讀「這一階段之前」才能落格後重跑
type: knowhow
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

產生程式要能在落格之後重跑：讀「這一階段之前」—— 組裝件 export 帶 hide_links=<本階段的關聯>，被本階段刻過的作品用 export mark=<本階段標記>:begin 讀；第一次跑時標記還不存在就退回現況。draft_relayout.py 這樣寫，落格後重跑：船尾門、陽台開口、梯子、6 門砲逐位元組相同。
🩸 沒這樣寫的話：第二次跑時船尾第 2、6 扇窗已經不是玻璃（找窗的斷言擋下）—— 跟階段十三 draft_guns.py「砲上船之後不能重跑」同一族。
