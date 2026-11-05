---
id: pitfall_terrain-window-dependence
topic: blocksail-world-globe
title: terrain.js 框大框小會變（REACH 外的最近島嶼）—— 一塊單獨算＝整片一起算要量
type: pitfall
status: active
created_at: 2026-11-05
created_by: unknown
links: []
related_docs: []
---

terrain.js 的「框大框小都一樣」以前只對距離成立：dist() 離岸超過 REACH（8 km）時距離封頂、hitSeg（最近哪一段岸）卻照留 ⇒ 粗格延伸得夠遠才看得到的那段岸，它那座島的礁、淺灘參數套進 8 km 外的海底。多佛海峽一塊 512 m：框往北多 2 km，289 點裡 129 點不同、最淺 18.8 ⇒ 7.2 m。2026-11-05 修（main de03fc9f；Tim 拍板 A），改完全部變深（導航網 6,656 塊、4 m 證言 27.2 萬點、0 變淺）。

怎麼抓到的：導航網要一塊一塊照用，Tim 的驗收格寫「先證實一塊單獨烘跟整格一起烘逐位元組相同」—— 證不過才追到。排除順序：載入順序（只載這一塊 vs 載整格：0 點不同）⇒ 找最近線段那張粗格的大小（固定成整格：差得一模一樣 ⇒ 不是它）⇒ 視窗往四個方向各多 2/4/8 塊二分 ⇒ 只有一個方向、2 km 起差 ⇒ 追一點的中間值（terrain 複本裡 hook：dc、hitSeg、cr、isn）。

⇒ 任何「切成一塊一塊算 ＝ 整片一起算」的說法，用 navmesh_bake --solo N ＋ 量具 bincmp.cjs 證，不要照 terrain.js 的註解相信。量具在 session 4710cd4a scratchpad globeperf/（winprobe*.cjs、tracept.cjs、bincmp.cjs、mindiff.cjs、deep4m.cjs）。
