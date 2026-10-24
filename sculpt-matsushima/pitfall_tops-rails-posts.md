---
id: pitfall_tops-rails-posts
topic: sculpt-matsushima
title: 桅樓要自己一層甲板、平台別開洞、細欄杆不反光、撐柱先量船寬
type: pitfall
status: active
created_at: 2026-10-24
created_by: ridge-001
links: []
related_docs: [senate:Docs/Workflows/Sculpture_Shipbuilding.md]
---

- 桅樓（或任何不在甲板上的小平台）上的砲要自己一層 kind=deck（level 比艦橋高、floor 寫平台的高度）：遊戲照甲板分砲甲板、一層一個高度帶；掛在艦橋底下 ⇒ 送藥手在兩個高度之間直線飛（Tim 2026-10-24「送藥手取彈藥會浮空」）。
- 繩梯、梯子兩頭都要落在宣告了 kind=deck 的甲板上，尋路才收；繩梯被丟掉匯入檢查**不報**（刻意的）⇒ 自己用 __game.ladders(i) 或 sh.ladDrop 看。
- 平台的取用點：彈藥庫 meta hoist 寫那座梯子（繩梯寫 ratlines 組名，例 main-lower-S；沒有物件 id ⇒ 匯入檢查報 ⚠ 路徑寫法，可以接受）。
- ⛔ 迴旋砲（mount=turret）的平台別開洞：砲班站位跟著砲繞砲軸轉，砲尾兩側那一圈就是他們站的地方 ⇒ 開人孔，砲班、送藥手踩空掉下去。
- 1 格細的欄杆標 iron 也看不出金屬：只有平滑面反光 ⇒ 要金屬就做 2 格粗＋render=smooth（混合材質壓重量）。
- 撐柱先量船殼在那一層的半寬（內傾）：艦橋翼端比船寬，柱子照翼端擺就懸在船外。
