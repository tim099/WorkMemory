---
id: pitfall_rails-wall-top
topic: sculpt-nightlamp
title: 欄杆找舷頂：每一層 z 各自找最外的牆，排除艙口圍欄與 tumblehome
type: pitfall
status: active
created_at: 2026-10-14
created_by: basecamp
links: []
related_docs: []
---

在舷頂上補東西（欄杆、立柱）時「舷頂在哪」要每一層 z 各自找：取那一層離最外那一格 ≤ 6 格的實心，而且離整欄最寬那一格 ≤ 20 格。
v1 拿整個半舷最高的實心 ⇒ 前甲板的艙口圍欄比舷頂高，欄杆爬到圍欄上；v2 拿「整欄最寬 − 6」⇒ 船尾的牆往內收（tumblehome），上半截不算牆，欄杆掛進船尾樓、立柱拉很長。
兩次都是先把結果畫成平面圖＋立面圖才看到的 —— 元件數、鏡像、浮空這些讀數兩版都是好的。
