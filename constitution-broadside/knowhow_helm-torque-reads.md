---
id: knowhow_helm-torque-reads
topic: constitution-broadside
title: 操舵力矩讀舵的哪幾格：份量與還剩幾成，不讀舵面大小、max_deg
type: knowhow
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

操舵併進動力（TASK-0777，dev 40693e0；31_drive.js helmItems／helmEff／helmYaw、31_sailing.js）：舵那一項只讀「份量」（meta helm_share，沒標就平分）與「還剩幾成」（舵、舵輪／舵柄、舵索串聯，受損乘算）。轉多快的上限是 31_sailing.js 那條 max(RUD_LOW, 0.012 ＋ 0.016·|航速|／SPK) × 舵效 × 上風係數 × sh.cfg.agile × 娛樂加成 × 天氣 —— **舵面多大、max_deg 都沒讀**。
⇒ ame 的可換零件把別艘的舵裝上來（例：夜隼號的小舵裝到憲法號），照程式現在轉得一樣快（讀程式推的，沒實測換過；2026-10-21 噗浪已這樣回 ame）。要讓換舵有差，得先拍「舵面積怎麼換成倍率」—— 新規格，要 Tim 決定。
開關 ?helmtq=0 ⇒ 整套照舊（反向對照 dump 逐字相同）。讀數 __game.helm(i)：eff／effC、R[] 每支舵的 w、ok、Fy。
