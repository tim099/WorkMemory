---
id: pitfall_upidle-idle-cap-hides-input-value
topic: blocksail-production
title: 缺料停工的建築照滿產算 ⇒ 值傳不到缺的料（AUTO.upIdle）
type: pitfall
status: active
created_at: 2026-11-06
created_by: unknown
links: []
related_docs: []
---

缺料停工的既有建築，自動建築以前看不到它們「少產的那一截」（TASK-1187，ridge-001 2026-11-03）。

- `AUTO.idleCap` 把缺料、賣不出價的建築照滿產算進產能 ⇒ 產品的缺口看起來是 0 ⇒ 往上游追的值傳不到缺的料。
- 量過：聖地牙哥 16 座煉鐵場等礦，礦坑一輪只值 0.2～4 錢；人手全被伐木場、金礦、餐館搶走，第二年礦坑照樣 1 座。
- 解法 `AUTO.upIdle`（world/auto.js idleOf）：停工的既有建築當成上游追的候選，值 ＝ 少產的 × 市價，只算市場真的缺的那一截（缺口＋停工少產的加回來）。
- 只開 upIdle（不開 `AUTO.imp`）時聖地牙哥鐵鏈跟 main 同一級 ⇒ 拉動鐵鏈的是 imp（跨城傳缺口）；upIdle 讓 imp 傳到的值走得進礦坑。
- 2026-11-06（TASK-1188 讀數）：船具行停工現在多半是缺木板（哈瓦那第 10 天停工時鐵件還有 10 桶，工地一天用 7 桶木板），不是缺鐵件 —— 下一個缺口從木板追。
