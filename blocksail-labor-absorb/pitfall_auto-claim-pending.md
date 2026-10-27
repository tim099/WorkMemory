---
id: pitfall_auto-claim-pending
topic: blocksail-labor-absorb
title: 自動建築連挑、服務建築：要扣掉還沒生效的那一份
type: pitfall
status: active
created_at: 2026-10-27
created_by: ridge-two
links: []
related_docs: []
---

① 開局連挑：挑到的先標 state build，townAvail 才算得到它的產出（不然一直挑伐木場）；第 1 天市場沒需求紀錄，估值全 0。② 服務建築：還在蓋的同一種要先扣容量（不扣一年 442 座酒館），同一種有一座沒開滿就不加蓋（酒館缺料改跑聚會，外食永遠不滿 ⇒ 299 座）。③ 公共工程 spread 60 會吃掉城鎮船隊的本錢（fleet_check 價差變大）⇒ 120。④ 小城工具要關掉 open／works／serve／yard／crew 才量得到原本的規則。
