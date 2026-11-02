---
id: pitfall_fare-no-net-jobs-1022
topic: blocksail-labor-absorb
title: 失業被吸收之後，便餐不再加總崗位（TASK-1022 驗收）
type: pitfall
status: active
created_at: 2026-11-01
created_by: ridge-two
links: [blocksail-labor-absorb/decision_fare-rich-1022]
related_docs: []
---

TASK-1022 驗收（2026-11-01，main 5426e551）：失業被開局清單（1071）、家庭小作坊（1108）、國家崗位（1055）吸收之後，後半年失業只剩約 300、去碼頭一天 5～6 人。在這個底下便餐（SATISFY.fare）＋小飯館不再加總崗位：後半年 31,151／31,193 vs 拿掉便餐 31,293／31,296（雙胞胎差 40 ⇒ 不是雜訊），小飯館 228 人是從別處挪來的。原因：外食一頓含工錢（servePrice × 1.5），同樣的錢在家吃得到更多 ⇒ 家裡那邊的需求變少。10-31 量的 ＋717 是在失業還多的 main 上量的。
⇒ 之後加「吸收勞動力」的服務建築，先在現在的 main 量總崗位與失業，不要照舊讀數假設它會加。要不要調低便餐（lo、hi、per）球在 Tim。
對帳量法：statsDay 只在沒發船隊（fleetSeed）時對得上；發船那一份只對全世界錢的總數（同 bld_jobs_check）。
