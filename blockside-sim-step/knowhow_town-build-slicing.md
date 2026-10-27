---
id: knowhow_town-build-slicing
topic: blockside-sim-step
title: 城鎮背景建造照段建（地面、實例、燈、換區）
type: knowhow
status: active
created_at: 2026-10-27
created_by: unknown
links: []
related_docs: []
---

Blockside 城鎮的背景建造照段建（2026-10-26，TASK-0817；63_towns.js）：共用 townEach（generator 裡 yield* townEach(L, f, k)，每 k 項讓出）。
- 地面 townGroundGen（Q.job）、實例 townInstGen（T.ij，每格至少 TOWN_INST_SLICE 4 ms，照 0819 不等預算）、燈 townLightGen＋townLightPut（T.lj[S／D]）、街燈位置 townPostsGen；一次建完的包裝 townGroundTile、townInst、townPosts 留著（測試、換模型組 townKitSet）。
- 規則：開始建就清 dirty／旗子（建到一半又要重建 ⇒ 建完再排一次）；建好才換上、舊的照畫；規劃又改了（instRedo）、模型組換了、實例換了 ⇒ 重來；townReplace 會把 instRedo、ij、tlS、tlD、lj 帶到新的 T。
- 一道牆 townWall 不拆（門塔轉角、牆接建築都在裡面，牆接建築讀地塊 ⇒ 不照城區快取）⇒ 長牆一格 20～30 ms 是剩下的。
- 換城區（fullNew 為 false）不算 townNeed、townPrep(p, keep) 沿用清樹（ox／oy／kx 一樣時）。
