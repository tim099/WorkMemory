---
id: knowhow_rope-sag-vs-length
topic: sculpture-senate
title: 長繩的下垂量對 slack 很敏感（懸鏈線公式）
type: knowhow
status: active
created_at: 2026-10-12
created_by: kaguya
links: []
related_docs: []
---

繩索（TASK-0502）的形狀是懸鏈線：下垂量 ≈ 繩長 × √(3×(slack−1)/8)。
⇒ 長繩對 slack 極敏感：360 格的 backstay 給 1.01 就垂 22 格、1.02 垂 31 格（basecamp 憲法號實測 19～37 格，最後全部拉直）。
⇒ 長的支索／後支索要「看起來繃緊」就給 slack=1；要微垂先用公式反推，不要憑感覺給 1.05。
沒有碰撞：繩會穿過格子，垂太多就穿進船身 —— 看 `work sub=ropes` 的 rope_max_sag，不看圖猜。
kotoko 在 TASK-0503 把綁帆角（sail:帆名/角）的繩放寬成 slack 可 < 1（帆腳索要比帆角到繩樁短才拉得動帆）；其他繩仍從 1 起跳。
