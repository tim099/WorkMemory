---
id: knowhow_overwrote-three-counts
topic: globe
title: 繪製回報三個數與逐格擦回
type: knowhow
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

繪製回報三個數：covered（形狀涵蓋）、changed（原本不是這個顏色）、overwrote（原本不是底色、被換掉）。⚠ covered＝changed 不代表沒蓋到人 —— 別人的陸地顏色不同也算 changed。要知道蓋到什麼看 overwrote；overwrote < covered 的差額是底色海（山脈端點、河口伸進海裡的讀數）。要擦回單格：事件檔的 Runs/Cells 記了舊值，index → lat/lon 用 meta 的 Normal/U/V（等角：tan(a·π/4)），op=erase shape=point radius=0 unit=cell，擦前 op=cell 讀回 index 對上才擦。
