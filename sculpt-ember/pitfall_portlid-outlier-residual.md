---
id: pitfall_portlid-outlier-residual
topic: sculpt-ember
title: 砲門蓋板凸出物要照擬合平面殘差判；擋路的錨鏈板刻掉一段
type: pitfall
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

砲門蓋板 portlids.py（basecamp-frigate-constitution-01m/design/tools，三艘共用；gura、kotoko 也抄去用）判「蓋板範圍裡有東西凸出來」：
- ⛔ 以前照「離中位數 > 3 格」⇒ tumblehome 收得多的那一段，蓋板上緣離中位數 4 格就被當成凸出物跳過（憲法號、餘燼號下層 07 號；殘差其實 0.66 格）。
- 現在先平面擬合、用「離擬合平面的殘差 > 3」判（Sculpture 267383b）；改完原本的 72 片擺法逐筆不變、只多出被誤判的那幾門。
- 真的擋住的多半是錨鏈板（channels 往下伸的鐵條切過蓋板邊緣）：Tim 10-20「遮擋到的部分可以移除」⇒ carvevox（帶顏色守門）刻掉蓋板範圍每邊多 1 格、上面多 4 格（開著往外翻那一段）裡的格；channels 子作品一舷、另一舷鏡像 ⇒ 刻一次兩舷都改。
- 找擋路的東西：關聯外框幾乎都是整艘船那麼大，判不出是哪一件；改畫「從船外正面看、每格凸出量上色」的圖（scratchpad lids/view_block.py 的做法）一眼就看得出是錨鏈板。
- 跳過的理由要寫量到的東西（殘差幾格、哪幾欄），⛔ 不要寫猜的物件名（上次寫「舷梯踏板」是補的住址）。
