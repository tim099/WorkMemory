---
id: pitfall_mirror-handedness
topic: constitution-broadside
title: 兩艘船一直是作品的鏡像（格與世界的手性；名字對不上先懷疑轉換）
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: [constitution-broadside/pitfall_models-hp-powder-traps]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13（ArtGallery 25ab475）：兩艘船一直是作品的鏡像。

- **症狀**：餘燼號的船尾名牌 EMBER 在遊戲裡反著寫；同一塊用雕刻引擎 view（eye=-150,161,160 target=100,161,150）讀起來是正的。
  憲法號左右對稱看不出來，唯一的痕跡是「S 在畫面上是左舷」—— 10-12 當成命名問題，在 sideName 把 S 叫成左舷蓋過去。
  ⛔ 名字跟畫面對不上時，先懷疑轉換（手性），不要先改名字。
- **原因**：雕刻引擎的格是另一個手性（面向船頭 +x、上 +z，+y 在右手邊 ＝ 右舷 S）；shipBaseMatrix 以前把格 y 轉成 −Z（旋轉）。
  改成 +Z（鏡射，行列式 −S³）。
- **鏡射會咬的地方**：
  · 繞序：主畫面畫船殼與帆要 frontFace(CW)（帆的著色器靠 gl_FrontFacing 翻法線）；反射那一趟兩次鏡射抵銷 ⇒ CCW。世界座標的東西（碎屑、粒子、海）照常 CCW。
  · 在船體座標裡手算的號都要翻：進水傾斜（32_hull 公式、33_flood floodTilt）、開砲後座、自動操舵的 sg（31_sailing）、進水細節頁的往哪一舷與俯視剖面。
  · 走 sh.M／sh.inv／M4.dir 的不用動；全在船體座標裡的（海浪縱搖橫搖、HUD 左低右低）也不用動。
- **驗法**：矩陣行列式要是負的；M4.dir(sh.M, 0, 1, 0) 跟「船頭方向的右手邊」內積要是正的；電腦對電腦 60 秒距離保持在 DIST 附近（自動操舵的號錯了會兩艘背對背開走）；有字的作品（名牌）從船尾看要讀得正。
- 戰局整個會變（風與浪沒有跟著鏡射），縮圖 72e49f1d → c72dfae8；不比勝負，比節奏。
