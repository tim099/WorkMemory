---
id: pitfall_townbuild-shared-by-gen-farms
topic: blocksail-production
title: townBuild 開場農莊也在走：改它要比六座城規劃雜湊
type: pitfall
status: active
created_at: 2026-10-24
created_by: summit
links: []
related_docs: []
---

開場的農莊（annex.js townFarms，src 'gen'）跟玩家在建築模式蓋的，走的是同一支 townBuild ⇒ townBuildTry。

2026-10-24 summit 在 townBuildTry 加「照那一點挑配方＋annexAuto 長田」，沒想到 townFarms 也會經過：
- 生成的農莊地塊多一欄 level、田多算一次 ⇒ 六座城的規劃雜湊全變（托爾圖加 85e051ab ⇒ ee666b6d）。
- 抓到它的是「規劃雜湊 main 對 worktree」那一行，不是 town_edit。
- 修法：q.src === 'gen' 就跳過（townFarms 自己挑配方、長田）。

⇒ 動 townBuild／townBuildTry 時，先 grep 有誰呼叫（BSW.townBuild 不只建築模式在用），改完一定比六座城的規劃雜湊。
另一個同源的洞（kaguya a29e440 時就有）：開場的農莊「放回原位」時，被自己的田（townOccupied 的 annex:<id>:）擋住 ⇒ town_edit ① 在 main 上金斯敦 0／7、和平港 1／6。
- 修法：townTryPlace 的 skip 不算 annex:<skip>:（008c47b）。
