---
id: knowhow_far-district-harbour
topic: blocksail-europe-ports
title: 有港口的遠城區（倫敦 Blackwall，TASK-1227）：berthMax 要自己寫、townDocks、地形框、驗法
type: knowhow
status: active
created_at: 2026-11-06
created_by: unknown
links: []
related_docs: []
---

TASK-1227（2026-11-05，Tim 結單）倫敦的碼頭遠城區 Blackwall —— 第一個「有港口的遠城區」。下一座要做的（例：塞維亞的桑盧卡爾外港、阿姆斯特丹的泰瑟爾錨地）照這幾條走：

- 資料：城區 `plan` 寫自己的 `berthMax`。城區沒寫的欄照城（`UNIT_FROM_CITY`）⇒ 主城寫了 small，遠城區不寫就跟著 small、大船泊位 0（Blackwall 第一次就這樣，生成器改成在河中補錨地，看起來像「有泊位」）。
- 泊位：`node tools/town_dock.cjs --town <城>~<區> --write 1` 寫進那一區的 plan.dock／docks；讀的地方一律 `BSW.townDocks(城)`（berth 會變成「<區>/<id>」）。挑泊位主城區先（Tim「小船優先倫敦池」）：globe dockOf、fleetfine fineBook、navgraph netLane 三處。
- 地形：遠城區常落在城那一框局部等高線外面 ⇒ 世界地圖的台地＋河岸陡坎，`town_link_draft` 說「找不到 ≤ 18° 的路」。先用 hline 探高度（直線上每 100 m 的 terrainGrid），再把 `tools/relief_rings.py` 那一框延過去（圖磚通常已在共用 _cache）。⚠ 框邊漸變 500 m 不要落在城區裡；兩框相鄰會在交界留一條原高度的脊 ⇒ 延同一框，不另開。
- port_draft：遠城區要從它自己的玩家停靠處量航道（已改），不然量到的是主城那段窄河。
- 順序：dock＋links 寫完 ⇒ town_bake --all（約 30 分鐘、全部過期是因為 areas.js／town.js 也改了）⇒ 反向對照另開 detached worktree 在 main 全烘一次比雜湊 ⇒ 開場檔：main 上那座城三項本來 ✓ 才能 `town_start --write 1` 兩次、只留那座城的檔 ⇒ nav 一串（nav_bake 最後一步 30 多分鐘）。
- 遊戲裡驗：藏著的預覽窗格裡，城鎮模式切城區（#tuiDK ⇒ data-dist）不生效、焦點留在船停的那一區 ⇒ 主城區只建骨架、拍不到；要拍別區走 Electron capturePage 那一套。
- fleet_check 跑一年 > 20 分鐘；改挑法的反向對照直接拿 netLane 新舊各抽 300 個鍵逐城比（42,000 次只有倫敦不同）。
