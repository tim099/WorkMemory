# 工作記憶索引 — constitution-broadside

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_blockside-coding-scope** — 改 Blockside 先開 Coding 場：要 build 的人 scope 含 index.html，範圍重疊就開 worktree  ↔ constitution-broadside/pitfall_shared-worktree-mixed-build
- **decision_blockside-pack-fingerprint** — 遊戲獨立成 Blockside、成品預設壓縮、.vxk 內容指紋與換機器 cache
- **decision_gunner-ammo-by-aim-zone** — 砲長照瞄準位置挑彈＋艦長指揮砲長
- **decision_helm-two-layer** — 艦長開船兩層：艦長只給期望航速、航速調節挑帆與俥鐘
- **decision_keymap-system** — 鍵位系統（66_keys.js）：動作表＋三個模式＋三層預設
- **decision_mixed-material-gun-hp** — 混合材質（厚度乘數＋線性混合、matMap）與砲血量公式
- **decision_pack-grid-mirror-diff** — 成品壓縮：格子鏡射＋diff（以及帆、網格、貼圖的量測）
- **decision_sickbay-musket-20261020** — 開打先上台子與火槍命中率：本小姐定的參數（待 Tim 看）
- **decision_staffing-bench-oar** — 設施配員、木工台、划槳傳輸鏈、報錯回報（Tim 拍板）

## knowhow
- **knowhow_blow-style-fall** — 被打掉的演出 blow=fly｜fall、帆桁停在船上要記撐著的那一格
- **knowhow_burn-douse-controlled-test** — 著火撲不滅的原因、救火修法與受控實驗的量法
- **knowhow_crew-fill-bearno-300s** — 量補位要看戰報 bearNo、打 300 秒（120 秒量不到）
- **knowhow_drive-tube-borrow** — 傳動件穿過船殼那一段借船殼的格；拆出模型會把它截斷
- **knowhow_furniture-models-from-sculpture** — 道具換成雕好的模型：furnmodels.py ⇒ 37_furnmodels.js、繞向與置中
- **knowhow_galley-walk-lateen** — 槳帆船：甲板 meta walk（中央走道）與三角帆桁 rig=lateen
- **knowhow_headless-async-audio-lab** — 無頭驗非同步結果：DevTools 等 Promise、__game 要 debug=1、環境音量 RMS
- **knowhow_headless-game-measure** — Blockside 無頭量測：?debug=1、run 與 live 的差別、閉包函式的 hook
- **knowhow_helm-torque-reads** — 操舵力矩讀舵的哪幾格：份量與還剩幾成，不讀舵面大小、max_deg
- **knowhow_input-controllers** — 控制器 71_ctrl.js 與輸入對拍腳本
- **knowhow_lineclear-relative** — 讓開砲線要看相對砲線的速度（射手與目標內插）
- **knowhow_maelstrom-architecture** — 大漩渦怎麼接進遊戲（VX 單一入口、參數表、量法）
- **knowhow_next-batch-plan** — 下一批：降旗設定、人手史料化、裝填公式、長砲損壞分級、站位修正（含史料出處）  ↔ constitution-broadside/state_wip-2026-10-12
- **knowhow_oar-test-ship** — 槳的量測：測試槳船怎麼做、示範模式不吃鍵盤、划槳方向用數字驗
- **knowhow_perf-budget-16ships** — 16 艘時每格畫面的成本分布、量具（rprof／frames）、三個減負的開關  ↔ pitfall_stale-vxk-cache
- **knowhow_report-mailto-discord** — 錯誤回報：mailto 2083 字上限、預設郵件程式壞掉 ⇒ 改送 Discord webhook
- **knowhow_review-findings-2026-10-12** — 審查確認的 32 項（位置、問題、修法；行號照 54be17d）  ↔ constitution-broadside/knowhow_next-batch-plan
- **knowhow_rooms-ropecheck-tools** — 艙房、掛著的模型、繩的檢查：新量具（__game 與 ropecheck.py）
- **knowhow_sculpt-regen-sync-models** — 重跑雕刻產生程式後同步：已拆成模型的格子別補回去（只刻／精確濾）
- **knowhow_shadow-2026-10-21** — 陰影現況：單張預設、分段貼合畫面、畫質頁細項、樹的邊界
- **knowhow_sinkflood-cost** — 沉船進水給人看：花費跟水量走、同時最多 2 艘、斷船那一截拿掉
- **knowhow_trailer-probe-survey-frames** — 宣傳片逐格讀狀態、定期檢查一幀幾 ms 的量法
- **knowhow_turret-layer-pitfalls** — 砲塔層會咬人的幾格（開放砲盾要平台、砲班黏著砲、彈藥箱連鎖、框只包砲塔）

## pitfall
- **pitfall_cache-prefs-traps** — 設定存檔與船型快取的坑（_cache 換船、只存非預設、壞存檔別順手蓋、快取鍵、驗證法）
- **pitfall_clew-tops-compass-stall** — 帆腳、桅頂、手機羅盤、頂風失速的坑（canvas 預設 300×150、帆腳跟 sheet 那根、離線模型調參）
- **pitfall_commander-line-sandbox-cal** — 指揮官戰列排不起來、沙盒校準的數字從哪來
- **pitfall_eval-write-per-match** — 長評測要打完一場寫一場
- **pitfall_flag-on-gaff-floats** — 旗掛 gaff 頂上，gaff 轉了旗沒跟
- **pitfall_flash-per-magazine** — 火光竄下艙口照取用點算 ⇒ 一次爆炸算兩次（娛樂火藥的讀數都偏）；先後要分清楚  ↔ constitution-broadside/pitfall_models-hp-powder-traps
- **pitfall_gpu-arrays-midline-comment** — 多船 GPU 改貼圖陣列、補丁插註解吃掉半行、sed 變 LF
- **pitfall_hands-swim-cutaway-traps** — 預備水手、剖面、落水求生踩過的坑
- **pitfall_hp-hit-once-and-traps** — 耐久／血量的坑（同一顆彈只算一次、長砲門檻不能壓、爆裂彈直擊重複、預設值對拍法、立繪去背參數）  ↔ constitution-broadside/state_wip-2026-10-12k
- **pitfall_hud-jitter-and-alpha-grid** — HUD 數字抖動與半透明格子疊出網格紋（固定欄寬、模擬層平均、小圖縮放貼上）
- **pitfall_import-reformats-game-json** — admin.py import 會把整份 game.json 重排 ⇒ 照 HEAD 只插入新船那一段
- **pitfall_load-deco-race** — 載入到一半換畫質「裝飾」⇒ T.h 載入失敗
- **pitfall_looks-right-2026-10-14** — 照字串找到註解裡的標籤、建置包進別人沒提交的設定、看起來在其實 alive=false 的燈、heredoc 跳脫變 NUL
- **pitfall_measure-and-impl-traps** — 量具盲區與實作坑（rAF 暫停、sandbox 截圖、--list、IIFE 互蓋、無錨物件）
- **pitfall_measure-stale-product** — 行內註解吃掉程式、建置失敗卻對拍舊成品、機器滿載時的量法
- **pitfall_midline-comment-eats-rest-3** — 行中 // 註解吃掉後半行（第三次，mountStep）—— eaten.py 沒抓到、node --check 抓到
- **pitfall_mirror-handedness** — 兩艘船一直是作品的鏡像（格與世界的手性；名字對不上先懷疑轉換）  ↔ constitution-broadside/pitfall_models-hp-powder-traps
- **pitfall_models-hp-powder-traps** — 長砲改模型、實際血量、殉爆：踩到的坑
- **pitfall_multiship-buoyancy-traps** — 砲長、減重上浮、多船架構：踩到的坑（縮圖 hash 對拍、浮力倍率不沉、MessageChannel…）
- **pitfall_perf-slowmo-traps** — 卡頓與殉爆慢動作的坑（從外面計時猜錯、斷船太早、水花不能照畫面走、量具）  ↔ pitfall_shreds-rain-traps
- **pitfall_phx-rng-traps** — 物理效果分級的坑（碎木共用亂數、帆與繩關會改戰局、帆的錯開奇偶、量物理成本）  ↔ constitution-broadside/pitfall_sea-gfx-traps
- **pitfall_plurk-response-lag** — 噗浪回應送出 200、回讀 0 則：對面晚了兩三分鐘才寫進去
- **pitfall_prop-iron-armor** — 會轉的體素件照鐵甲算擊穿 ⇒ 一格都打不掉
- **pitfall_push-dev-local-ahead** — 推 dev 前本機 dev 要等於遠端；管線會吞掉 ff 失敗
- **pitfall_rooms-hud-ropes-traps** — 艙房、HUD、浮空繩這一天的坑（同名函式、canvas 定位、shrink-to-fit、錨不是綁點）
- **pitfall_rope-end-guess-by-distance** — 繩的端點照距離猜綁在哪，猜到旁邊的模型就跟著斷（哨兵號繩梯、憲法號 shroud）
- **pitfall_sea-gfx-traps** — 海浪與畫質管線的坑（彈簧週期、bob 的意思、重複紋路、VoxelKit 著色器、陰影取樣器、MSAA、量效能）  ↔ constitution-broadside/pitfall_clew-tops-compass-stall
- **pitfall_sea-level-zero-assumptions** — 寫死海面＝0 的地方：水面一凹下去就靜默出錯
- **pitfall_shared-worktree-mixed-build** — Blockside 共用工作區：在工作區建的成品會包進別人施工到一半的改動（Tim 測到的「卡死」）
- **pitfall_shreds-rain-traps** — 帆的布片與 3D 雨滴的坑（平行截縮圖船沒載完、背景分頁雨不走、布片落太快）  ↔ pitfall_water-flood-traps
- **pitfall_split-physics-traps** — 拆檔與物理的坑（template 不能直接開、檢查掃註解、-I 忽略編碼、物理不抽亂數、布看約束數）
- **pitfall_squash-onto-moved-main** — 壓縮分支時 reset --soft 到「現在的 main」會把別人新合的倒回去
- **pitfall_stale-diff-baseline** — 對拍基準會跟著預設值過期：對拍前用當時的 dev 重產基準  ↔ constitution-broadside/pitfall_measure-stale-product
- **pitfall_stale-vxk-cache** — _cache 的 .vxk 落後 ⇒ 成品默默換成舊船；反向對照要拿「上一版提交的成品」比  ↔ pitfall_ui-hud-relayout-traps
- **pitfall_taskkill-chrome-too-broad** — 停無頭 Chrome 不要照程式名殺 —— 照 profile 路徑篩
- **pitfall_ui-hud-relayout-traps** — 介面重排一整串的坑（無頭最窄 500、rAF 背景停、innerHTML 重畫吃 click、帆雙層抵銷、?cmd 讀太早、固定風偏一邊、拿掉文字前先想用途）
- **pitfall_water-flood-traps** — 船邊水花與進水粒子的坑（白沫池滿、船頭浪被蓋、照樣抽亂數、設定檔汙染縮圖、粒子進水少的原因、自由液面）
- **pitfall_window-keydown-capture** — 同掛 window 的 keydown：stopPropagation 擋不住；data-k 圓鈕不要再掛 onclick
- **pitfall_worktree-crlf-build** — Blockside 新 worktree 是 CRLF：game.json 內嵌進成品 ⇒ 同一個 HEAD 建出不同的 index.html  ↔ constitution-broadside/decision_blockside-coding-scope
- **pitfall_wrapup-0514-202610121204** — 收工紀錄 TASK-0514：《憲法號對轟》開發進度

## state
- **state_fps-2026-10-16b** — 第一人稱（TASK-0649）第四版：碰撞三段、跳得進洞、過梁低頭；量的時候會咬人的  ↔ constitution-broadside/state_fps-2026-10-16
- **state_wip-2026-10-13d** — 2026-10-13 上午：七筆提交（含手機羅盤排版、追砲彈鏡頭），0520 已結；0514 剩美術與重量整場；0522–0526 不動工  ↔ constitution-broadside/state_wip-2026-10-13c
- **state_fps-2026-10-16** — 第一人稱（TASK-0649）告一段落：現況、還開著的四件、量的時候會咬人的 ~~[superseded]~~  ↔ constitution-broadside/decision_blockside-coding-scope, constitution-broadside/pitfall_worktree-crlf-build, constitution-broadside/state_fps-2026-10-16b
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
- **pointer_external-shell** — 外掛外殼 admin.py shell 與 Blocksail battle.html 的重套規矩
- **pointer_ladder-points-meta** — 梯子點：kind=ladders＋foot／lo／hi／head（兩艘船）
- **pointer_review-2026-10-12** — 審查清單與子單在哪  ↔ constitution-broadside/knowhow_next-batch-plan
