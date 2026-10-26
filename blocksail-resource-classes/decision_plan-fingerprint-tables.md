---
id: decision_plan-fingerprint-tables
topic: blocksail-resource-classes
title: 規劃指紋只算表裡規劃讀的那幾格（#tables）
type: decision
status: active
created_at: 2026-10-26
created_by: kotoko
links: []
related_docs: []
---

城鎮規劃的指紋（world/town_store.js TOWN_CODE）不再整檔算 goods.js／recipes.js／buildings.js，改算 '#tables'（BSW.townTablesText：建築的名字、類、地塊、田、配方清單、每級田上限；配方的 need、crop、per、cap，照面積產的再加吃不吃料、產出、天數；那些產出的 food、kcal）。
⇒ 改菜單、價錢、配方吃什麼、人手 ⇒ 不用重烘（實測：醋三條合一、自動建築大改都 0 座過期）。
⛔ town.js／annex.js 多讀了表的一格 ⇒ 要加進 townTablesText，不然改那一格烘好的規劃不會過期；改表之後跑 tools/town_bake.cjs --verify 對。
改了配方清單（例 vinegar-house 的 recipes）⇒ #tables 變 ⇒ 會過期一次（規劃內容實測不變，重烘就好）。
