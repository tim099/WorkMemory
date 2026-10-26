---
id: pitfall_jobless-vs-market-price
topic: blocksail-production
title: 失業多、田沒人：市場價卡住（開場糧食灌太多、開場需求 0）
type: pitfall
status: active
created_at: 2026-10-26
created_by: gura
links: []
related_docs: []
---

「失業很多、建築又沒人」先查市場價，不是查崗位分配（2026-10-26，君士坦丁堡）。
- 判定照設計：失業 ＝ 16～55 歲 − 分到崗位；缺工只算 want（照市場價想雇的）− w ⇒ 賣不回成本少雇的崗位**不算缺工** ⇒ 會出現「失業 1,648、缺工 0、154 座田 0 人」。
- 量法：抓 prodDay 的 rep.b（townDay 把它傳給 BSW.siteDay，包一層就拿得到），照 why／rMkt／rL／rIn 分桶算崗位；再拆 ρ ＝ 產值 ÷（工資＋原料）照基準價、照市場價各算一次。工具在 gura session 76d69531 scratchpad pinas/jobs_probe.cjs、d_probe.cjs、town_series.cjs。
- 坑 ①：開場基礎糧食照人口全灌成自己城的主糧 ⇒ 君士坦丁堡 32,657 桶菜（實際吃菜速度 160 天份）⇒ 菜價地板。改成每種最多「自己城滿產 FOOD_BASE_PROD_DAYS 天」。
- 坑 ②：開場每種貨 d ＝ 0 ⇒ 曲線 ref 只剩 refMin 10 桶 ⇒ 第一週全部地板價（酒 3／30）。第 0 天照人口填主糧的 d。菜在麵包吃完前量到的需求也是 0（人先吃麵包）。
- 曲線（Tim 拍板）：基準價 ＝ 成本 × 1.5；30～60 天份平的＝基準價，180 天份＝成本，再多照天數慢慢跌；生產 ρ ≥ 0.8 照樣滿產、≤ 0.4 停。換配方估堆貨照舊用跌得快的那段（marketPrice steep），不然第二座也去搶同一個缺口（recipes_check 反向對照）。
- 改起始檔只改市場主糧的 s、d：讀 world/start → 改 → savePack → 逐檔比「只有 market.goods.<主糧>.s／.d 變」→ 再跑一次 0 變（腳本 food_start_delta.cjs）。
- 還在的：君士坦丁堡 16～55 歲 2,900 人 vs 生產崗位約 1,400 ⇒ 全雇滿也上千人沒工作，人口一年流失 4～5 成 —— 結構性，要從開場建築／自動建築調。
