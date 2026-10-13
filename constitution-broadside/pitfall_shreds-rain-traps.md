---
id: pitfall_shreds-rain-traps
topic: constitution-broadside
title: 帆的布片與 3D 雨滴的坑（平行截縮圖船沒載完、背景分頁雨不走、布片落太快）
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: [pitfall_water-flood-traps]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13 晚上（帆的布片 34_shreds.js、3D 雨滴 14_rain.js）踩到的：

- **四個無頭 Chrome 同時截 ?demo=75 縮圖 ⇒ 有一張船沒載完**：截出來只有海和天（hash 不對，像是改壞了）。單獨重跑就跟 HEAD 一樣。縮圖對拍一張一張跑（--dump-dom 量讀數可以平行，那個不看畫面）；hash 對不上先打開圖看是不是空的。
- **跟 HEAD 比要另外 build 一份 HEAD**：`git archive HEAD HtmlGames/_build/ame_constitution_broadside VoxelKit | tar -x`，再把 VoxelKit/_cache 複製過去（沒進版控），build 到 HtmlGames/_headtest.html 截完就刪。不要 stash 工作區（summit 的 Comic 檔也在髒）。
- **預覽分頁在背景時 rAF 停 ⇒ 每格畫面走的東西（3D 雨滴）根本沒在跑**：G.run 推模擬、雨 n ＝ 0，對拍出來「逐字相同」是假的。要用 __game.rain({ bench: n }) 每 0.1 秒手動插格，讀數裡看到雨的落海次數才算數。
- **布片照帆的風壓會掉太快**：KW 攤平落下 4.4 m/s、一片 1.5 m 的布 2 秒從 14 m 落海（每秒 5–9 m），不像在飄 ⇒ 布片的風壓 ×3（c.kw，clothStep 裡乘；帆沒設 ＝ 1）。
- **一發就藏掉 30–40 m² 的帆**（tearCloth：碰到斷掉約束的粗格點，從它內插的細格點都藏）⇒ 布片一發十幾片；布片數照物理分級設上限（每艘 150／80／40）。
