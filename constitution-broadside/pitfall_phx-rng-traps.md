---
id: pitfall_phx-rng-traps
topic: constitution-broadside
title: 物理效果分級的坑（碎木共用亂數、帆與繩關會改戰局、帆的錯開奇偶、量物理成本）
type: pitfall
status: active
created_at: 2026-10-13
created_by: unknown
links: [constitution-broadside/pitfall_sea-gfx-traps]
related_docs: []
---

2026-10-13（TASK-0526 物理效果分級）：

- **「只給人看的」東西別抽戰局的亂數**：碎木以前用 rnd()（生的時候抽轉軸，stepDebris 每顆落水、落地也抽）⇒ 少生一顆碎木，後面的亂數全部錯位、整局都變。現在碎木走 rndD（12_effects.js），開局跟 rs 一起重設。煙與水花的參數是呼叫端抽的，spawnPt 裡面只用節拍跳過 ⇒ 少生不改戰局。之後加新的效果（水粒子、進水粒子、天氣）一律走自己的亂數或節拍，⛔ 不要碰 rnd()。
- **帆與繩關掉會改戰局**：帆桁只在物理開著時追風，帆桁的角度改到打中帆桁的那幾下 ⇒「低」（帆與繩關）跟全開走出不同的一局。這是原本就有的；反向對照要拿「全開但帆與繩關」跟「低」比（逐字相同）。
- **帆的錯開步數保持奇偶**：原本 (si+k+i)&1，改成 (si+k+i+1)%every===0 ⇒ every＝2 時同一批步數，高 ＝ 原本逐步相同（⛔ 換成 %2===0 ⇒ 帆走另一批步、最後一格的布不同、縮圖變）。
- **量物理的成本別用整格**：整格（一步＋畫一格）是畫面佔大頭，各檔差在雜訊裡；用 G.run(4)（240 步只畫一格）除 240，再看 __game.prof() 的 cloth／rope。量的那幾秒如果碰上桅斷、殉爆，那一檔的數字不能比。
