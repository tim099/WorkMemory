---
id: knowhow_battle-flag-recipe-pins
topic: blockside-render
title: 戰鬥裡的船旗變形、不會動：先查作品配方的 pin 與第 4 角
type: knowhow
status: active
created_at: 2026-11-02
created_by: erina
links: []
related_docs: []
---

**戰鬥裡的船旗變形、不會動 ⇒ 先查作品的帆配方（pin、第 4 角），不是遊戲的布**（erina 2026-11-01；信風號、好旅號）

- 我艦（戰鬥模式）的旗走布料物理：靜止形狀 ＝ 作品配方生的網格，綁法 ＝ 配方的 pin／bind（29_sailrecipe.js clothBindMeta）。表演船走循環布（floopAuthFlag 另算旗形），看起來正常不代表作品沒問題。
- 標準寫法（全庫 34 面船旗掃過）：`free=[1,2]` ⇒ pin [T,F,F,T]、旗邊 ＝ 飛邊的長方形。掛帆桁的旗：第 4 角寫成 `model:<帆桁>/<peak 的模型座標往下 eh>`（三叉戟號），旗邊垂直、整條跟帆桁轉。
- 信風號、好旅號以前：free=None（四角全綁，飛邊兩角釘在原地 hk 3）＋第 4 角綁帆桁往前那一點 ⇒ 梯形、旗尾 6 秒位移 0。改完：綁 7 點（旗邊）、旗尾有在飄。Sculpture b82f9d1c／4f968076，Blockside a81f8c3／47938ad。
- 模型座標 ＝ 來源作品的格座標；世界 ＝ at ＋ R·(scale ⊙ (p ＋ 0.5 − pivot))。有轉的帆桁（好旅號三角帆桁 Ry 50°）用兩個已知固定點反驗旋轉方向。小數座標匯出時會取整（差 7 cm）。
- 2026-11-01 起帆不限作者（Senate 88e91ac，GameLayer 記 meta_log）⇒ 別人的船旗也能直接改。
- 量具：`__game.cloth(i, '<帆名>')` 回那一面的 p／p0／hk／yk／uv；掃配方用 admin.vxk_header 讀 _cache 的 .vxk。
- 截圖：瀏覽器窗格藏著時畫面不更新 ⇒ 用螢幕外 Chrome＋CDP；船在動，截圖前 `__game.speed(0)`、截完 speed(1)。
