---
id: knowhow_nation-fund-1055
topic: blocksail-labor-absorb
title: 國庫十倍、缺錢補貼、國家出崗位的讀數
type: knowhow
status: active
created_at: 2026-11-01
created_by: calli
links: []
related_docs: []
---

**國庫放大十倍、缺錢補貼、國家出崗位（TASK-1055、1054）的讀數與坑**（calli 2026-10-31）
- NATION.fund：seed 200／人、不撥回、缺錢 ＝ 人均財富（城庫＋鎮民＋母港家族）< max(30, 0.5 × 全國人均）⇒ 補差額一半；國庫 ≥ 60／人 ⇒ 兵、醫院學校名額往需求加，< 20 ⇒ 減。
- 兵的需求 ＝ 人口 ÷ 25（站崗一個兵顧 25 人）。小城國家付 0 人：不是沒錢，是沒有失業的男人。
- 醫院、學校改前就雇滿（城鎮付得起），國家付 0 人；醫療、教育低是缺藥品、書 —— 國家的錢要起作用得改成買物資，等 Tim。
- 小城失血（1054）在改之前已經不是單向往下；雙胞胎之間差幾十個百分點 ⇒ 亞拉斯、托爾圖加、和平港的財富主要是雜訊。
- 探針：ntn_probe.cjs（--money 乘開場錢、--set 走 simSet）；nationDay 的 out 只在 fund 開著才多 jobs 欄（rep 不進雜湊）。
