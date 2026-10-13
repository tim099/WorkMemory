---
id: pitfall_hands-swim-cutaway-traps
topic: constitution-broadside
title: 預備水手、剖面、落水求生踩過的坑
type: pitfall
status: active
created_at: 2026-10-13
created_by: unknown
links: []
related_docs: []
---

2026-10-13 預備水手／自爆／剖面／落水求生（ArtGallery e464b9a、822bdc2）踩過的坑：
- 走路格的相鄰高低差只給 2 格 ⇒ 憲法號砲甲板 x 570 那一階 0.3 m 把前段整片切掉，前面兩座梯子接不上；改 3 格。
- 梯子 head 吸到走路格只看平面距離 ⇒ 吸到艙口圍欄頂上（z 85 而不是 82）；高度差要加權。
- 挑梯子不算排隊 ⇒ 開戰第一輪 26 人擠同一座後梯子，10 秒只補上 2 個。
- 新加的 sampler 沒指定單元 ⇒ 預設第 0 號撞到畫布，整次繪製被丟（Feedback loop 警告 200+），分支沒走到也一樣；驗證要開新分頁（舊分頁警告有上限）。
- 剖面／拿掉甲板時水線下照染海水色 ⇒ 沒進水的船底看起來積了一層水（Tim 抓到）。
- reset 沒清慢動作 ⇒ 殉爆／自爆後重開，新的一局接著慢、鏡頭被拉回上一局（ec301ae 起就有）。
- 船速不限就能攀船 ⇒ 7.7 節推 6 個下海爬回 5 個；加 3 m/s 門檻。
- 艙內淹水當落水 ⇒ 人穿過船殼浮到海面再游回來；改成預備水手往上一層躲。
- 換邊邊跑 forEach 邊 splice ⇒ 跳過下一個人；改成每一步跑完再換。
- 送藥手掉下海沒清 g.pm ⇒ 換到敵艦後仍「能動」，原本那幾門砲沒人送。
- 量測：bash heredoc 會把 \n 吃成 \n（patch 檔改用 Write 寫或 r-string）；hud() 每 6 次才重畫；Tab 鍵切的航行模式重開會保留（量停船時要重新載入）。
