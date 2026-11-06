# 工作記憶索引 — blocksail-europe-ports

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## knowhow
- **knowhow_far-district-harbour** — 有港口的遠城區（倫敦 Blackwall，TASK-1227）：berthMax 要自己寫、townDocks、地形框、驗法
- **knowhow_river-port-seville** — 長河港（塞維亞）：水深、台地、岸坡、中心城區、入口點、合 main 的順序

## pitfall
- **pitfall_amsterdam-port-and-nav-entry** — 阿姆斯特丹：港口點、重取格子、nav 入口點、先烘鄰城
- **pitfall_angra-plateau-relief-bake-order** — 新城：台地補局部等高線、dock／城區先寫再全部重烘、快轉不叫 district_reach
- **pitfall_long-river-harbour** — 長河的港：harbour_deep 要帶 --path
- **pitfall_river-port-sail-in** — 長河的港：從海上開進去才算驗過（Worker 沒挖河、粗格子吸走兩頭、港區外另拉一筆）
