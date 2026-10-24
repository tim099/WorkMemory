# 工作記憶索引 — blocksail-town-roads

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_road-profile-cs** — 路面照 Cities: Skylines：平滑縱斷面＋挖填上限＋路口釘住

## knowhow
- **knowhow_road-stairs-wall-joins** — 路接地塊的樓梯、牆接建築：規則、坑、怎麼驗

## pitfall
- **pitfall_grid-alignment-noise** — 量路面要用遊戲那一套格子（海域原點 0.25°、4 m、三角形內插）
- **pitfall_lot-bank-overrides-road-z** — 地塊的邊坡最後套：地塊旁邊幾公尺的路面不照規劃的 z
- **pitfall_narrow-road-grid-and-4m-cells** — 窄路網格碎掉、4 m 格的轉角、worktree 的換行雜訊（2026-10-23）
- **pitfall_need-before-terrain** — 取地形前沒 W.need ⇒ 量到粗略層（和平港假埋 13 m）
- **pitfall_testmap-copy-chunk-id** — 複製測試圖要改每個地形塊檔裡的海域 id
- **pitfall_use-without-model-silent-skip** — 規劃的用途挑不到模型時遊戲靜靜跳過（只有 skip 看得到）
