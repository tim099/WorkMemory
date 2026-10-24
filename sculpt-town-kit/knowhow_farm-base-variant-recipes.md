---
id: knowhow_farm-base-variant-recipes
topic: sculpt-town-kit
title: 農莊與種植園：基礎款＋換作業區、照配方挑（recipes／townRecipe）
type: knowhow
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

農莊與種植園（Sculpture meadow-farm-01m，BUILDING_SPEC §6.20，2026-10-24）：「基礎款＋換子模型」的做法 —— build.py 多 share（那一層關聯到基礎款那一件子作品、不另存；verify 記 same_as_base），作業區層 kit.PRIO['work']=2 比牆身低一級 ⇒ 變體碰不到共用的牆身。
照配方挑：成品牆身 meta recipes ⇒ townmodels 寫進索引 recipes ⇒ Blockside townRecipe（63_towns.js；town_top.js 手抄同一條）。候選都沒寫 recipes 的用途原封不動。別的生產建築要「照配方換一款」只要寫 meta recipes。
坑：毛石牆 4 格厚，put_part 的 t 要給牆厚（照木板牆給 2 ⇒ 窗沒挖穿、屋裡封成 3 萬格空腔）；屋簷會把板條縫封成空腔；斜線小件要六連通（rope_line）；遊戲裡 188 是螢光綠。
驗法：scratchpad 腳本攔 fs.readFileSync 讓 tools/town_foot.cjs 讀別的 Blockside 樹 ⇒ 六座城烘好的規劃逐塊看挑到哪一件；遊戲裡 __globe.place + sailTown + tick 停靠、#sail 進城、iframe 加 &debug=1、畫面用 snap sink 收。
