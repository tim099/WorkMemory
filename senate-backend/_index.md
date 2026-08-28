# 工作記憶索引 — senate-backend

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_decisions-d1-d10** — 十條拍板（含兩條被實測改掉的）
- **decision_git-layer-port-2026-08-26** — git 管理層移植 SCP_Core：四顆拆法、宿主注入、CLI 寫入端不給預設對象
- **decision_submodule-page-decide-half-2026-08-28** — Submodule 頁補齊「決定」那半（逐項設定→編譯成指令）＋CLI --set-branch＋SCP_Ui.ToggleValue  ↔ senate-backend/decision_git-layer-port-2026-08-26

## knowhow
- **knowhow_first-background-job-and-host-redraw** — 本 repo 第一個背景工作：執行緒契約六條＋RedrawsContinuously＋兩段式確認要住 session  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28
- **knowhow_imgui-clipboard-bridge** — ImGui 剪貼簿 callback 接法：八條判準＋三層驗收（第三層刻意留白）  ↔ senate-backend/pitfall_typed-field-per-char-rescan-and-clipboard
- **knowhow_ui-driver** — UI 有四種驅動方式，任兩種互為證人

## pitfall
- **pitfall_pitfalls-day1** — Day 1 撞到的六個坑（都不會當場叫）
- **pitfall_prefix-branch-rules-host-half-missing** — 啟發式家規那一半宿主從沒宣告（UCL_→Dev）＋repo 目標改可直接打路徑  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28
- **pitfall_silknet-imgui-no-modifier-keys** — Silk.NET ImGuiController 從來沒送 modifier ⇒ 所有 Ctrl 快捷鍵無效（打字正常）＋ keydebug 診斷基建  ↔ senate-backend/knowhow_imgui-clipboard-bridge
- **pitfall_typed-field-per-char-rescan-and-clipboard** — 打字欄位逐字元重掃＋生效值跨 process 丟失＋ImGui 吃不到 Ctrl+V（全站）  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28

## state
- **state_state-day2** — Day 2 現況：顯示參數／頁面堆疊／反射三層都上了，Unity 端仍零讀數  ↔ senate-backend/state_state-day1
- **state_state-day1** — Day 1 現況：能跑能看能操作，三格未驗 ~~[superseded]~~  ↔ senate-backend/state_state-day2
