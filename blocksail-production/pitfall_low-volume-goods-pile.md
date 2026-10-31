---
id: pitfall_low-volume-goods-pile
topic: blocksail-production
title: 低量加工品會在做它的城一直堆：refMin 墊底＋PROD_MKT 賠著做
type: pitfall
status: active
created_at: 2026-10-31
created_by: trailhead
links: []
related_docs: []
---

新加的低量加工品（一座城一天才買 0.01～0.1 桶）照預設會在做它的城一直堆（TASK-0992，2026-10-31 量）：
- 市場曲線的參考量 ＝ max(理論消耗 × 30 天, refMin)，MARKET.refMin 預設 10 桶 ⇒ 小城要堆到約 60 桶價才跌破成本。
- 開工率 PROD_MKT（world/prod.js）：賣價 ≥（工資＋原料照基準價）× 0.8 就滿產、≤ 0.4 才停 ⇒ 賠著做也照做。
- 解法：goods.js 給那種貨 refMin（同砲、幣）—— 鞋、銅器、陶菸斗 refMin 1 之後，兩年末庫存 鞋 150 ⇒ 106、銅器 64 ⇒ 39、陶菸斗 405 ⇒ 266；第二年還在慢慢漲（PROD_MKT 的門檻沒動）。
- 量法：跑兩年、看每季末庫存（一年只看年底會漏：第三、四季才開始堆）；每座城的 產／買走／理論／價 一起看。
- 一年的經濟模擬也晃 ±3～8%：改前、改後各配一組只動 MARKET.ema 0.3 ⇒ 0.3001 的雙胞胎，兩組 after 都超出雙胞胎的才算。
- 新增 food 4 的貨會分走奢侈品那一籃一份（蜜餞：菸葉、薑、蘭姆酒各 −5～−8%）；新增吃銅錠的配方會讓船隊改運別的、鑄幣少（銅匠鋪：銅幣 −16～−22%）。
- 探針：session 5065a8ce 暫存區 c0992/probe.cjs（每季庫存、每城讀數、建築 type×recipe 計數；PT=貨,… 加逐城）、stop_probe.cjs（缺料／缺人／堆貨的反向）、build_probe.cjs（建築模式試放，同 town_edit ⑥）。
