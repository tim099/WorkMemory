---
id: pitfall_list-fields-cobweb
topic: blocksail-labor-absorb
title: 開局清單的田第一個播種月一起換作物：蛛網效應的失業潮（TASK-1136）
type: pitfall
status: active
created_at: 2026-11-01
created_by: Abigail
links: [blocksail-labor-absorb/pitfall_region-shared-gap]
related_docs: [D:/Unity/Blocksail/docs/PRODUCTION.md]
---

開局清單在第 2 天一口氣蓋好的幾百座田，配方是照很空的需求紀錄挑的；到第一個播種月（約第 91 天）全部看到同一個價、一起換作物（蛛網效應）⇒ 舊規則每座停工 60 天、不雇人 ⇒ 5,878 人同時沒工作、第 91～180 天走掉 3,136 人。讀數只看前 90 天會完全看不到。
TASK-1136 的解：SWITCH.grow（換作物那 60 天照雇人、付工資、不產）＋ SWITCH.share 0.25（同時換的田最多兩成五、至少一塊）。只開 grow 不開 share ⇒ 小城（太子港）主人一次換好幾塊、60 天沒收成照付工資 ⇒ 錢燒光、田賣給城鎮、城鎮也付不出 ⇒ 饑荒。
量經濟一律跑兩年、配微擾雙胞胎（錢 × 1.0001 或 buf 0.21）；第二年各城的去碼頭是混沌的，比前 90 天與兩年後的人口（雙胞胎差 100～300 人）。scratchpad 實驗台 bal_lab.cjs、sw_probe.cjs（每 15 天換配方次數與停工人手）、pdp_probe.cjs（盯一座城的糧食、錢、田）。
