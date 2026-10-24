---
id: knowhow_farm-base-share-recipes
topic: sculpt-town-kit
title: 農莊模型：基礎款共用、作業區換款、照配方挑
type: knowhow
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

農莊與種植園的模型（Sculpture meadow-farm-01m；BUILDING_SPEC §6.20；2026-10-24 meadow）：基礎款 farm、plantation 的牆身／屋頂是共用的同一件子作品，各款只換作業區那一件。
產生程式 design/build.py 多兩格：share（那一層關聯到基礎款那一件，verify 的 same_as_base 逐格對照）、meta（recipes）。作業區層優先序比牆身低（kit.PRIO['work'] = 2）。
遊戲照配方挑：成品 meta recipes ⇒ townmodels 索引 recipes ⇒ Blockside townRecipe（town_top.js 同一條）；沒寫 recipes 的候選原封不動。
要給別的生產建築做「照配方換一款」：寫成品 meta recipes 就好。驗挑法：照 town_foot.cjs 抽 Blockside 的 townPick 跑烘好的規劃（world/townplans/*.json）。
坑：毛石牆 4 格厚，部件要照牆厚挖（t=4），不然窗沒挖穿、屋裡封成空腔；甘蔗 188 在遊戲裡是螢光綠。
