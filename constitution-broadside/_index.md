# 工作記憶索引 — constitution-broadside

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_blockside-coding-scope** — 改 Blockside 先開 Coding 場：要 build 的人 scope 含 index.html，範圍重疊就開 worktree  ↔ constitution-broadside/pitfall_shared-worktree-mixed-build
- **decision_blockside-pack-fingerprint** — 遊戲獨立成 Blockside、成品預設壓縮、.vxk 內容指紋與換機器 cache
- **decision_pack-grid-mirror-diff** — 成品壓縮：格子鏡射＋diff（以及帆、網格、貼圖的量測）

## knowhow
- **knowhow_lineclear-relative** — 讓開砲線要看相對砲線的速度（射手與目標內插）
- **knowhow_next-batch-plan** — 下一批：降旗設定、人手史料化、裝填公式、長砲損壞分級、站位修正（含史料出處）  ↔ constitution-broadside/state_wip-2026-10-12
- **knowhow_perf-budget-16ships** — 16 艘時每格畫面的成本分布、量具（rprof／frames）、三個減負的開關  ↔ pitfall_stale-vxk-cache
- **knowhow_review-findings-2026-10-12** — 審查確認的 32 項（位置、問題、修法；行號照 54be17d）  ↔ constitution-broadside/knowhow_next-batch-plan
- **knowhow_rooms-ropecheck-tools** — 艙房、掛著的模型、繩的檢查：新量具（__game 與 ropecheck.py）

## pitfall
- **pitfall_cache-prefs-traps** — 設定存檔與船型快取的坑（_cache 換船、只存非預設、壞存檔別順手蓋、快取鍵、驗證法）
- **pitfall_clew-tops-compass-stall** — 帆腳、桅頂、手機羅盤、頂風失速的坑（canvas 預設 300×150、帆腳跟 sheet 那根、離線模型調參）
- **pitfall_commander-line-sandbox-cal** — 指揮官戰列排不起來、沙盒校準的數字從哪來
- **pitfall_flag-on-gaff-floats** — 旗掛 gaff 頂上，gaff 轉了旗沒跟
- **pitfall_flash-per-magazine** — 火光竄下艙口照取用點算 ⇒ 一次爆炸算兩次（娛樂火藥的讀數都偏）；先後要分清楚  ↔ constitution-broadside/pitfall_models-hp-powder-traps
- **pitfall_gpu-arrays-midline-comment** — 多船 GPU 改貼圖陣列、補丁插註解吃掉半行、sed 變 LF
- **pitfall_hands-swim-cutaway-traps** — 預備水手、剖面、落水求生踩過的坑
- **pitfall_hp-hit-once-and-traps** — 耐久／血量的坑（同一顆彈只算一次、長砲門檻不能壓、爆裂彈直擊重複、預設值對拍法、立繪去背參數）  ↔ constitution-broadside/state_wip-2026-10-12k
- **pitfall_looks-right-2026-10-14** — 照字串找到註解裡的標籤、建置包進別人沒提交的設定、看起來在其實 alive=false 的燈、heredoc 跳脫變 NUL
- **pitfall_measure-and-impl-traps** — 量具盲區與實作坑（rAF 暫停、sandbox 截圖、--list、IIFE 互蓋、無錨物件）
- **pitfall_mirror-handedness** — 兩艘船一直是作品的鏡像（格與世界的手性；名字對不上先懷疑轉換）  ↔ constitution-broadside/pitfall_models-hp-powder-traps
- **pitfall_models-hp-powder-traps** — 長砲改模型、實際血量、殉爆：踩到的坑
- **pitfall_multiship-buoyancy-traps** — 砲長、減重上浮、多船架構：踩到的坑（縮圖 hash 對拍、浮力倍率不沉、MessageChannel…）
- **pitfall_perf-slowmo-traps** — 卡頓與殉爆慢動作的坑（從外面計時猜錯、斷船太早、水花不能照畫面走、量具）  ↔ pitfall_shreds-rain-traps
- **pitfall_phx-rng-traps** — 物理效果分級的坑（碎木共用亂數、帆與繩關會改戰局、帆的錯開奇偶、量物理成本）  ↔ constitution-broadside/pitfall_sea-gfx-traps
- **pitfall_rooms-hud-ropes-traps** — 艙房、HUD、浮空繩這一天的坑（同名函式、canvas 定位、shrink-to-fit、錨不是綁點）
- **pitfall_sea-gfx-traps** — 海浪與畫質管線的坑（彈簧週期、bob 的意思、重複紋路、VoxelKit 著色器、陰影取樣器、MSAA、量效能）  ↔ constitution-broadside/pitfall_clew-tops-compass-stall
- **pitfall_shared-worktree-mixed-build** — Blockside 共用工作區：在工作區建的成品會包進別人施工到一半的改動（Tim 測到的「卡死」）
- **pitfall_shreds-rain-traps** — 帆的布片與 3D 雨滴的坑（平行截縮圖船沒載完、背景分頁雨不走、布片落太快）  ↔ pitfall_water-flood-traps
- **pitfall_split-physics-traps** — 拆檔與物理的坑（template 不能直接開、檢查掃註解、-I 忽略編碼、物理不抽亂數、布看約束數）
- **pitfall_stale-vxk-cache** — _cache 的 .vxk 落後 ⇒ 成品默默換成舊船；反向對照要拿「上一版提交的成品」比  ↔ pitfall_ui-hud-relayout-traps
- **pitfall_ui-hud-relayout-traps** — 介面重排一整串的坑（無頭最窄 500、rAF 背景停、innerHTML 重畫吃 click、帆雙層抵銷、?cmd 讀太早、固定風偏一邊、拿掉文字前先想用途）
- **pitfall_water-flood-traps** — 船邊水花與進水粒子的坑（白沫池滿、船頭浪被蓋、照樣抽亂數、設定檔汙染縮圖、粒子進水少的原因、自由液面）
- **pitfall_wrapup-0514-202610121204** — 收工紀錄 TASK-0514：《憲法號對轟》開發進度

## state
- **state_wip-2026-10-13d** — 2026-10-13 上午：七筆提交（含手機羅盤排版、追砲彈鏡頭），0520 已結；0514 剩美術與重量整場；0522–0526 不動工  ↔ constitution-broadside/state_wip-2026-10-13c
- **state_wip-2026-10-12** — 2026-10-12 收工現況：哪些已提交、哪些在工作區 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12b
- **state_wip-2026-10-12b** — 2026-10-12 收工現況：已提交 54be17d，還沒做的在哪 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12, constitution-broadside/state_wip-2026-10-12c
- **state_wip-2026-10-12c** — 2026-10-12 下午：降旗設定、煙、帆布片做完（工作區未提交）；下一步在哪 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12b, constitution-broadside/state_wip-2026-10-12d
- **state_wip-2026-10-12d** — 2026-10-12 傍晚：降旗、人手集中、帆腳索、自動航行做完（工作區未提交）；手動航行提案等拍板 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12c, constitution-broadside/state_wip-2026-10-12e
- **state_wip-2026-10-12e** — 2026-10-12 傍晚：降旗、人手集中、帆腳索、自動航行做完（工作區未提交）；手動航行提案等拍板 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12d, constitution-broadside/state_wip-2026-10-12f
- **state_wip-2026-10-12f** — 2026-10-12 晚上：手動航行、兩舷各自開火、彩色砲列、硬度 20 做完（工作區未提交） ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12e, constitution-broadside/state_wip-2026-10-12g
- **state_wip-2026-10-12g** — 2026-10-12 晚上：＋航跡位置、遊戲內切換模式／鏡頭、A／D 照畫面、左下羅盤（工作區未提交） ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12f, constitution-broadside/state_wip-2026-10-12h
- **state_wip-2026-10-12h** — 2026-10-12 晚上：＋鏡頭跟我艦轉、自動開砲（觀戰）、航速／靈活性滑桿（工作區未提交） ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12g, constitution-broadside/state_wip-2026-10-12i
- **state_wip-2026-10-12i** — 2026-10-12 收工：f6274cd 已提交（航行、兩舷、降旗、羅盤、自動開砲…）；下一批在哪 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12h, constitution-broadside/state_wip-2026-10-12j
- **state_wip-2026-10-12j** — 2026-10-12 晚上（wake#13）：物理／拆檔／鏡頭／射界／全收在工作區未提交；下一步模型砲＋血條、彈藥庫殉爆 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12i, constitution-broadside/state_wip-2026-10-12k
- **state_wip-2026-10-12k** — 2026-10-12 晚上告一段落：細節設定／耐久／船員單圖與新立繪已提交；下一步實際血量（設計在 TASK-0514 #12） ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12j, constitution-broadside/state_wip-2026-10-13
- **state_wip-2026-10-13** — 2026-10-13 收工：砲長、減重上浮、多船三步都已提交；等 basecamp 餘燼號的砲 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-12k, constitution-broadside/state_wip-2026-10-13b
- **state_wip-2026-10-13b** — 2026-10-13 早上：TASK-0520 設定存檔＋船型快取已提交（748f279），剩手機量一次；仍等 basecamp 餘燼號的砲 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-13, constitution-broadside/state_wip-2026-10-13c
- **state_wip-2026-10-13c** — 2026-10-13 上午：設定存檔＋快取、帆腳、桅頂、手機羅盤、頂風失速＋玩法都已提交；0514 剩美術與重量整場；新開 0522–0526 不動工 ~~[superseded]~~  ↔ constitution-broadside/state_wip-2026-10-13b, constitution-broadside/state_wip-2026-10-13d

## pointer
- **pointer_ladder-points-meta** — 梯子點：kind=ladders＋foot／lo／hi／head（兩艘船）
- **pointer_review-2026-10-12** — 審查清單與子單在哪  ↔ constitution-broadside/knowhow_next-batch-plan
