---
id: knowhow_river-port-seville
topic: blocksail-europe-ports
title: 長河港（塞維亞）：水深、台地、岸坡、中心城區、入口點、合 main 的順序
type: knowhow
status: active
created_at: 2026-11-04
created_by: unknown
links: []
related_docs: []
---

塞維亞（TASK-1165，2026-11-03）長河港一路踩下來的順序；倫敦、阿姆斯特丹是同一個病。

1. **水深**：harbour_deep 加 --path（河窄處會斷開）、--shore 10 --ramp 15,40,80；格子 --r 到河口外、--step 64。
   - ⚠ 等深線只有 5／10／20 m 三層：depth 5 ⇒ 河底平平 5 m；depth 6～10 都會把 10 m 那一層彎進來 ⇒ 中線 10 m、往兩岸 30 m 內降到 7 m 以下。沒有「7 m」這種中間值。
   - 預設那艘風暴角號吃水 4.97 m ＋ 避淺 1.5 m ⇒ 要 6.5 m；5 m 的河地球儀排不出航線（sailTown 回 true 但 route 一直是空的，不報錯）。Tim 選加深。
2. **台地峽谷**：世界地圖在歐洲內陸幾十 km 常常整片 30～45 m 的台地，挖回的河變 40 m 深峽谷 ⇒ relief_rings 加一框（要下載 terrarium z10 圖磚，Tim 同意過）。
   - 只放 20／30／40（拿掉 10 m）反而更陡 —— 10 m 那一層要留。
3. **岸坡**：補完等高線岸邊還是 50 m 內爬 15 m ⇒ 生成器「港口街：從廣場走不到港口」⇒ 那條河 coast 的 bank 60 ⇒ 150 m（tools/europe_waters.py 的參數一起改；同泰晤士河）。bank 只動 index.js，不動 world/bathy/*.js。
4. **中心城區**：市中心離河 500 m ⇒ 自動長成鑰匙孔走廊 ⇒ 手畫 core（要碰到港口、包住 center；縮太小廣場卡在角上、港口街又斷）。
5. **港口型式**：150 m 寬的河用 skala（短棧橋 60 m），預設那套 88～92 m 會伸過半條河。
6. **入口點**：窗裡沒有開闊深水 ⇒ 舊版取柵格第一格（西北角）⇒ basecamp 9256b1ae 修了（far 落在淺死巷就改挑 bw0 ≥ 一半的格子）。外段最淺可能只有 4 m 上下，但地球儀照樣開得進去 —— 驗收一定要從海上實際開一次（place 在鄰港的停靠航道尾端 ⇒ sailTown ⇒ 等 docked 且 route 空）。
7. **合 main**：一天之內 main 被合三次（倫敦、阿姆斯特丹、南美、導航網）；每次都要：底稿補上別人新改的那幾格（git show <那一筆>^:world/bathy/<檔>）⇒ --apply ⇒ 還原漂移 12 檔 ⇒ splice index（多前綴：seville,guadalquivir）⇒ nav 三支重烘。nav 檔記得 git add（合併那一筆漏收過一次）。
8. world/navmesh.bin（TASK-1196）是建置產物：ff 之後在共用區 node tools/navmesh_bake.cjs 增量烘（474 格約 24 分鐘）。
