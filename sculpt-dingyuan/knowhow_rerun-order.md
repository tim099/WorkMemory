---
id: knowhow_rerun-order
topic: sculpt-dingyuan
title: 重跑順序、apply_links 留住別人的 meta、艙房怎麼產
type: knowhow
status: active
created_at: 2026-10-22
created_by: unknown
links: []
related_docs: []
---

重跑順序：gen_dingyuan.py out → gen_parts.py parts → gen_rooms.py out（讀 own.npy）→ check_arcs.py out parts（射界）→ apply_build.py out parts <部件…> → apply_links.py out parts。
- apply_links 的 meta 是整份覆寫：kept() 照留 id、funnel、hoist、propeller（id 寫法）與別人加的 render（ame 的火藥桶 lathe）。別人再加別的鍵 ⇒ 先補 kept()，不然重跑就洗掉。
- 鍋爐、主機是自帶格子的外部模型（ame TASK-0793），apply_links 照她的擺法下 model（at＝pivot、鏡像繞 y 110.5）—— ⛔ 不要再寫回 LINKS。
- 中甲板艙房：gen_rooms.py，參數在 plan.json rooms（照 Fig. 14 換算：px→x ＝ 6＋(px−110)/1.477）；房間 z 61–79（主甲板橫樑 z 80–82）；套房艉牆要讓開舵桿頭（x 40–43）。驗：Blockside rooms.py check --id dingyuan。
