---
id: decision_sickbay-musket-20261020
topic: constitution-broadside
title: 開打先上台子與火槍命中率：本小姐定的參數（待 Tim 看）
type: decision
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

2026-10-20（dev 1844803）本小姐定的兩個設計，參數在設定檔、Tim 還沒在測試站看過：
- 開打先上台子（44_medic.js，TASK-0568）：furniture.table hold 1、battle 1；rooms.medical.calm 60。交戰 ＝ 開戰後 calm 秒內，或海上任何砲（g.firedAt）／火槍（sh.mkAt）calm 秒內開過。交戰中 free 只收 battle 家具；砲停了病床開放（台子多算 MED_TBL 300 格）、台子上的人派預備水手抬去病床（job.move；治療進度帶著走、路上不治）。calm 0 或沒有台子 ⇒ 不分交戰。⚠ 只看砲的第一版被火槍先開打騙了（實測抓到）。代價：同一個種子搬下去 32 → 10。
- 火槍命中率（40_crew.js musketP／musketHit，TASK-0633）：s ＝ 距離 × √(sigma² ＋ (sway × 船轉速)²)，命中 ＝ erf(w÷2√2s)·erf(h÷2√2s)；musket {sigma 0.013, w 0.5, h 1.0, hTop 1.6, sway 0.15}。sigma 從 1811 年 Brown Bess 試射（100／200／300 碼 53／30／23%、板高 1.8 m）反推。一槍照舊一次 rnd()。不晃：25 m 49%、100 m 4.6%、300 m 0.5%。天氣沒算。
