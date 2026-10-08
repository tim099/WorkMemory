# 工作記憶索引 — senate-backend

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_decisions-d1-d10** — 十條拍板（含兩條被實測改掉的）
- **decision_doc-lives-with-cmd** — 文件住在指令所在那一邊；help 靠文件 frontmatter 的 cmds: 反查
- **decision_git-layer-port-2026-08-26** — git 管理層移植 SCP_Core：四顆拆法、宿主注入、CLI 寫入端不給預設對象
- **decision_senatedata-layout-and-single-exe** — 檔案版面收斂：SenateData 資料根 ＋ 執行檔只剩一顆 ＋ install 吃掉 setup
- **decision_server-identity-serverid** — Server 身分化（serverId）：main 逐字沿用舊路徑，其餘加後綴  ↔ tavern-senate-migration/decision_d10-explicit-writer-switch
- **decision_submodule-page-decide-half-2026-08-28** — Submodule 頁補齊「決定」那半（逐項設定→編譯成指令）＋CLI --set-branch＋SCP_Ui.ToggleValue  ↔ senate-backend/decision_git-layer-port-2026-08-26
- **decision_tavern-post-target-data-root** — Editor→Senate 發文用 target_data_root 選專案（不能叫 data_root）

## knowhow
- **knowhow_discord-mention-rewrite** — Discord 轉發 @ 通知：常駐送出才換 <@id>，手動補發不通知
- **knowhow_ff-senate-scpcore-locally** — Bar 的 SCP_Core 帶著別人未推的 commit 時，用本機 ff 把自己的 commit 送進 Senate 那份，不 push
- **knowhow_first-background-job-and-host-redraw** — 本 repo 第一個背景工作：執行緒契約六條＋RedrawsContinuously＋兩段式確認要住 session  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28
- **knowhow_globe-export-worldmap-v2** — 球面輸出：世界地圖投影與 export=1；順手撞到的工具行為  ↔ senate-backend/knowhow_globe-export-worldmap
- **knowhow_imgui-clipboard-bridge** — ImGui 剪貼簿 callback 接法：八條判準＋三層驗收（第三層刻意留白）  ↔ senate-backend/pitfall_typed-field-per-char-rescan-and-clipboard
- **knowhow_ui-driver** — UI 有四種驅動方式，任兩種互為證人
- **knowhow_globe-export-worldmap** — 球面輸出：世界地圖投影與 export=1；順手撞到的工具行為 ~~[superseded]~~  ↔ senate-backend/knowhow_globe-export-worldmap-v2

## pitfall
- **pitfall_assets-scp-core-binobj-cs1704** — Assets 底下的 SCP_Core 長出 bin/obj ⇒ CS1704 同名組件，把別人的 Unity 編譯弄紅（偵測一行；成因未解）
- **pitfall_build-window-in-agent-process-tree** — build 收尾那顆常駐視窗在 agent 的行程樹裡＝永不結束的 GUI 子行程（ClaudeCode 關不掉／重啟被占用）
- **pitfall_compile-status-freshness-scope** — compile-status 的新鮮度比的是最新那顆組件
- **pitfall_delete-block-hidden-public-member** — 刪共用區塊前逐個 grep 公開成員；recompile clean 要看 stale_sources
- **pitfall_dev-exe-autostart-readonly** — dev senate.exe 連只讀 Cmd 也會拉起 Server（0329 之後），鎖住 dll 讓下一次 build 失敗而你讀到舊 binary
- **pitfall_fixes-bypasses-criteria** — commit 帶 Fixes TASK-N 會直接把單推成 done，驗收格全空；結單走 op=resolve 且預設 dry-run
- **pitfall_gui-font-binding-traps** — Senate 視窗字型三坑：ImFontGlyph 版面錯、Silk BGRA、explorer 正斜線
- **pitfall_pitfalls-day1** — Day 1 撞到的六個坑（都不會當場叫）
- **pitfall_prefix-branch-rules-host-half-missing** — 啟發式家規那一半宿主從沒宣告（UCL_→Dev）＋repo 目標改可直接打路徑  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28
- **pitfall_registry-sanitizetag-eats-dot** — registry 的 SanitizeTag 把點改成底線 ⇒ 活著的 Server 被報成 not_running
- **pitfall_scp-core-stale-dll-on-no-build** — 改完 SCP_Core 用 --no-build 實跑會吃舊 DLL —— 而失敗樣子跟「我的 code 沒生效」同形
- **pitfall_silknet-imgui-no-modifier-keys** — Silk.NET ImGuiController 從來沒送 modifier ⇒ 所有 Ctrl 快捷鍵無效（打字正常）＋ keydebug 診斷基建  ↔ senate-backend/knowhow_imgui-clipboard-bridge
- **pitfall_submodule-save-applies-draft-20261007** — 儲存應驗證並納入當前路徑草稿
- **pitfall_typed-field-per-char-rescan-and-clipboard** — 打字欄位逐字元重掃＋生效值跨 process 丟失＋ImGui 吃不到 Ctrl+V（全站）  ↔ senate-backend/decision_submodule-page-decide-half-2026-08-28
- **pitfall_ui-driver-set-click-two-steps** — senate ui --set 與 --click 要分兩道
- **pitfall_verify-the-right-copy** — 驗到的不是改的那份：sed 換掉 CRLF、PATH 上是出廠版 senate

## state
- **state_state-day2** — Day 2 現況：顯示參數／頁面堆疊／反射三層都上了，Unity 端仍零讀數  ↔ senate-backend/state_state-day1
- **state_state-day1** — Day 1 現況：能跑能看能操作，三格未驗 ~~[superseded]~~  ↔ senate-backend/state_state-day2
