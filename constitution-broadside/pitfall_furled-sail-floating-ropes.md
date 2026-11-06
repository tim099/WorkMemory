---
id: pitfall_furled-sail-floating-ropes
topic: constitution-broadside
title: 收帆後 sheet、tack 浮在帆張開的位置：三條畫法有一條漏接
type: pitfall
status: active
created_at: 2026-11-05
created_by: unknown
links: []
related_docs: []
---

收著的帆、它自己的 sheet、tack 在 Blockside 有三條畫法，任何一條漏接就會「照作品張開時的位置畫」⇒ 帆腳那一點浮在半空（Tim 2026-11-04 百夫長號截圖）。
- 跟著帆腳畫：sailWalk 把 c.tt 的繩放進 tDraw／tJ（looseBuf 畫）；收好凍結（c.fz）時照凍結的帆腳。
- 藏起來：buildRig 對收著的帆把 s.lines 標 hide。
- 照 r.pts 畫：上面兩條都沒接到 ⇒ 作品張開的位置 ⇒ 浮空。
已修的兩個洞（Blockside dev 7bf4184）：① 收著的帆帆桁被打掉 ⇒ furlLost 把帆腳排出 tJ、buildRig 看 c.fz 不藏 ⇒ 改 furlCut 跟帆斷開垂下來；② buildRig 只看 PHYS、sailWalk 看 PHYS && !rigFixed ⇒ 城鎮模式／表演船收帆時浮 ⇒ 兩邊叫同一支 rigPhysOn。
還開著：三角帆的 tack 不在 tethOf（17 型），收帆後偏約 0.37 m。
量法：在 index.html 副本插 window.__EV（killCrew 前），包 upload 攔 buildRig 送去畫的靜態線段，找帆張開時的帆腳座標在不在裡面 —— 量的是畫出去的東西，不是重寫一次判斷。⚠ 新 helper 先 grep 撞名（51_captain.js 已有 rigLive）。
