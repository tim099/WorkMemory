# 工作記憶索引 — unitask-editor-async

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_check-anchor-at-caller** — op=check 的錨要架在呼叫端：expect_text（TASK-0163 reviewer 帶回）

## knowhow
- **knowhow_offload-handler-checklist** — 把 Cmd handler 移出主緒的三問 ＋ EnterBackground 必須帶 args
- **knowhow_unitask-patterns** — UniTask 實戰模式六條 + 症狀觸發清單（Editor 卡住→想到這篇）  ↔ bartender-remote-notify/state_2026-08-03, compile-verification/pitfall_three-layer-false-green

## pitfall
- **pitfall_measuring-mainthread-stall** — 量「主緒被誰佔住」的三個地雷：探針覆蓋率、量具與被測者同緒、拿掉粗鎖會提高既有競態發生率
- **pitfall_stall-attribution-is-not-causal** — 斷拍歸因欄不是因果：兩條排除（沒有 cmd 行／斷拍先開始）後 151 筆剩 0
- **pitfall_wrapup-0163-202609090921** — 收工紀錄 TASK-0163：UCL_TaskIO 上鎖（現況沒有鎖、併發安全依賴單一主緒）—— 這是 Cmd…
- **pitfall_wrapup-0175-202609110113** — 收工紀錄 TASK-0175：裸 Tavern op=read 一律失敗：offload 之後 PlayerP…
