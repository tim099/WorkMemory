---
id: state_abort-on-user-input
topic: bartender-remote-notify
title: 使用者操作即中斷（CTS 守衛＋合成輸入分帳），實測待 Tim
type: state
status: active
created_at: 2026-08-20
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/Bartender/UCL_RemoteWindowControl.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/Bartender/UCL_RemoteNotifyService.cs]
---

自動通知／群發的「使用者操作即中斷」（Tim 2026-08-20 拍板，未 commit）。

問題：既有 idle 護欄只擋流程**開始前**；流程中（Locate OCR 數秒＋各步 delay）使用者回來動鍵鼠，貼上與 Enter 照樣執行 ⇒ 訊息打進使用者當下的輸入框。

機制（中斷訊號用 CancellationTokenSource，Tim 指定）：
- `UCL_UserInputAbortGuard`（UCL_RemoteWindowControl.cs）：建立時記 LastInputTick 基準，`Poll()` 命中即 Cancel 自己的 CTS。**同步輪詢** —— 流程的 Sleep 是 Thread.Sleep 阻塞主執行緒，背景 task 沒機會跑。
- 關鍵分帳：自己 SendInput 的合成輸入也會更新 GetLastInputInfo ⇒ 低階發送統一走 `SendInputOwn`（發完蓋 s_OwnInputTick 戳記）；判定式＝最後輸入 > max(基準, 自有戳記)+120ms 容差。容差內的真使用者輸入會漏判（取捨：反向誤判會讓功能自斷）。tick 比較用 unchecked 減法防 49.7 天 wrap。
- `SleepInterruptible`：50ms 切片＋片間 Poll，取代序列中的三個 Sleep。
- 檢查點：Locate 前後／點擊前／貼上前／Enter 前（Enter 前中斷＝文字已輸入未送出，摘要照實講）。RunOnceCore 與 DeliverTextTo 兩份序列都裝（重複是已知債，守衛也兩份）。
- 中斷＝**視為未觸發**：不寫 state（不計 retry、不進冷卻），trace 記 `notify_aborted` 非 `notify_failed`（AbortRun/AbortRunWithSummary，與 Finish 分開）。
- 開關 `AbortOnUserInput` 預設開，persist 進 notify config（abort_on_user_input）；AdminPage 自動通知區塊有 ToggleLeft「使用者操作即中斷」。

驗過：recompile 0 errors；Invoke 煙測 LastInputTick=615923937、UserIdleSeconds=12.656（P/Invoke 路徑活）。
⚠ 未驗：真實中斷路徑零讀數 —— 要 Tim 開 remote-window、按「立即執行一次」後中途動滑鼠，預期摘要出現 ⏸ 且 trace 是 notify_aborted。
