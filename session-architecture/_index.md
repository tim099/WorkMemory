# 工作記憶索引 — session-architecture

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_ruling-c1-d1** — Session 統一架構五拍板（Tim 2026-08-26）
- **decision_ruling-coding-lease** — 拍板：Coding 場一律帶租期（(A)）＋三個細節；⚠ 而重複進場會把它抹掉  ↔ session-architecture/decision_ruling-coding-session-scope-a
- **decision_ruling-coding-session** — Coding session（Tim 2026-08-26 追加拍板）
- **decision_ruling-coding-session-scope-a** — 拍板：Coding session 射程走 (A) 兩宿主都納入（Tim 2026-09-05）—— 09-04 的成本估計被 0127 的地形推翻  ↔ session-architecture/pointer_port-0127-after-onecut
- **decision_ruling-ended-at-single** — settled_at/ended_at 判一個事件（PM 2026-08-26，採 summit 第一刀）
- **decision_ruling-port-to-scp-and-gateway** — 拍板：Session 搬 SCP_Core／頁面不保留／結算不搬走 gateway（含 TrySettle→TryClose 的語意修正）

## pitfall
- **pitfall_cross-process-delegation** — 跨 process 委派四格：Editor 開著變條件／回讀才是判定／UI 不能同步等／lane 與 target_persona
- **pitfall_dotnet-build-under-assets** — 掛在 Assets/ 底下的 csproj 不要 dotnet build —— CS1704 會報在無關的 assembly 上
- **pitfall_mechanism-exists-nobody-walks-it** — 機制在 ≠ 有人在走 —— 層綠而生產路徑沒接，而測試也是真的綠
- **pitfall_same-door-scope** — 「走同一個門」的兩次射程誤用 —— 不是所有路徑；而「同一個函式」≠「同一條路」  ↔ session-architecture/pitfall_mechanism-exists-nobody-walks-it
- **pitfall_second-path-setting** — 加一格路徑設定之前先問「這個值系統裡是不是已經存著了」—— 我造了第二份，而第一份就在旁邊
- **pitfall_side-action-before-main-action** — 附帶動作跑在主動作之前 —— 主動作失敗時它已經做掉了，而帳上留著一個沒發生過的事件
- **pitfall_wrapup-0054-202608270937** — 收工紀錄 TASK-0054：儲存統一：sessions/ 扁平路徑＋kind 入 json＋StreamWa…
- **pitfall_write-back-eats-unknown-keys** — 寫回吃掉 model 不認識的鍵／[NonSerialized] 沒用／全域 Factory 污染測試 —— 三隻都回綠

## state
- **state_chain-20260826-summit** — 鏈進度快照＋0054 開工三拍板（summit 晚安交棒）

## pointer
- **pointer_port-0127-after-onecut** — TASK-0127 ⑦ 之後的落點：session 層只剩 SCP_Core 一份（＋新增 kind 的 SOP、兩個地雷）  ↔ session-architecture/pointer_port-0127-entry-points
- **pointer_port-0127-entry-points** — TASK-0127 落點：新家／舊家／讀數指令，與動 ⑦ 前要知道的兩件事 ~~[superseded]~~  ↔ session-architecture/pointer_port-0127-after-onecut
