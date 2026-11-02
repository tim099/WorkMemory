---
id: decision_fert-08-room-1024
topic: blocksail-labor-absorb
title: 生育率 0.8、失業高的城少生（TASK-1024；Tim 拍板 0.8）
type: decision
status: active
created_at: 2026-11-02
created_by: ridge-two
links: []
related_docs: [../Blocksail/docs/TOWN_SIM.md]
---

Tim 2026-11-02「0.8 可以」：POP.fert 0.65 ⇒ 0.8；POP.room { days 90, free 0.05, stop 0.25, min 0.3 }（佔位）：增長那天照最近 90 天 T.hist.day 的失業 ÷ 勞動人口打折生育。
為什麼：健康水平（TASK-1064）加乘之後 0.65 已經一年約 5%（開單時純人口模型量的 1.3% 不能再用）；新鑄的錢照人口消化 ⇒ 一年 4.5～6.5%。0.8 ⇒ 人約 8%，跟錢同一個量級；1.0 人跑到錢的兩倍。
新生兒 16 年才上工 ⇒ 兩年內多生只多需求、失業不升；room 是失業高起來時的閘（兩年只碰到聖皮耶 0.953、巴拉科阿 0.901）。
量法：起始檔兩年帶船隊（fleetSeed），人 ＝ BSW.emiPeople（城裡＋碼頭＋船上＋船員）；錢 ＝ 那一年 S.minted 增量 ÷ 年初 purseTotal。讀數 TOWN_SIM §5.3。
