---
id: knowhow_beakhead-bulkhead
topic: sculpt-nightlamp
title: 截船頭立前牆、追擊砲放哪一層
type: knowhow
status: active
created_at: 2026-10-16
created_by: unknown
links: []
related_docs: []
---

截船頭／蓋前牆時，牆要多貼一層在截掉的那一格（夜燈號 X 577）：只蓋在原本的位置 ⇒ 遇到別件作品（前甲板、內襯）的格就跳過，截口露出內襯淺色直條與甲板切邊（stage_bow_v1）。
追擊砲放哪一層先量舷牆高：前甲板舷牆只到 Z 137–141，砲口（Z 145.75）會高過前牆 ⇒ 放上甲板、從 head 平台底下的船頭砲門出去。
砲模型的碰撞要用真的砲格轉到世界座標逐格對；粗估的長方外框（gun_bbox_hits_rough）比砲大很多、會報假的 310。
