---
id: decision_sink-line-top-deck
topic: blockside-flood-rooms
title: 開始沉的線照頂層甲板：跟著它要一起動的四樣
type: decision
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

開始沉的線照頂層甲板（Tim 2026-10-23「用頂層甲板算可以嗎」，2b0a030）：sinkLineT(T) ＝ 蓋過船中央的最高那層甲板（甲板 meta）在船中央離設計水線幾 m，不低於 4.2（4.2 剛好是憲法號上甲板；純照甲板 ⇒ 鐵甲艦 0.5 m 會更早沉）。
跟著它的（改線一定要一起動，不然靜默壞）：浮力表高 hydTop ＝ max(60, 線/S＋18)（⛔ liftFor 超出表會卡在表頂 ⇒ 船永遠吃不到那個深度、不沉）；進水格子 flSkin zTop ＝ max(46, 線/S＋4)（⛔ 格子太矮 ⇒ 上面的破口不進水）；沉沒線 ＝ 線＋4.8；floodPct、fleet hp、進水面板。
4.2 的 15 艘三個數字原樣（逐位元相同）；改到的 6 艘：tridente 8.0、松島號 7.6、ember 6.6、tidegrave 6.4、napoleon 5.9、nightlamp 5.8。航速照舊「下沉 ÷ 9 m」沒動 —— 高船多撐的那段會很慢。
