---
id: decision_height-frame-no-drag
topic: sculpt-frigate
title: 高度的框：11 號 body plan 沒畫 drag，sheer plan 讀數要換框
type: decision
status: active
created_at: 2026-10-11
created_by: unknown
links: []
related_docs: []
---

11 號（1927 MIT 線形圖）的 body plan 沒畫 drag（龍骨跟水線平行），sheer plan 有（0.88°，船尾深）。
本作品的船殼照 body plan 做 ⇒ 作品是「沒畫 drag」的那個框；sheer plan 上的高度要先換框才放得進來：船尾約 −5 格、船頭約 +3 格。
- 證據：船尾 counter 下緣 sheer 86.5 ↔ body 81.2（差 +4.0..4.8）、船頭 hawse 橢圓差 −3.6（正負相反）；body plan 各站在同一組水線高度上跟水線半寬圖一致到 0.1 格。verifier 85% 確認（2026-10-11）。
- 一致的換算：Z_作品 ＝ 62.86 ＋（sheer 上高於水線 A 的高度，用局部比例）× 3.048。⚠ 不要「均一比例的 0.01538·(439.2 − X)」再乘局部比例 —— 那是比例修正算兩次（差到 0.6 格）。
- 影響：砲門、甲板、舷弧不要直接拿 sheer plan 相對龍骨的高度；02 號（1849）也是相對龍骨畫的 ⇒ 階段二的艉柱頭 Z 88 在本框約 81–83（伸出 wing transom，雕兩頭前要刻掉 81–87）。
出處：作品 notes.md「階段六」、plan.json ends.provenance.frame_of_heights、source_ends_raw.json。
