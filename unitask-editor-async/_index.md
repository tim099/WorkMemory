# 工作記憶索引 — unitask-editor-async

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_check-anchor-at-caller** — op=check 的錨要架在呼叫端：expect_text（TASK-0163 reviewer 帶回）

## knowhow
- **knowhow_unitask-patterns** — UniTask 實戰模式六條 + 症狀觸發清單（Editor 卡住→想到這篇）  ↔ bartender-remote-notify/state_2026-08-03, compile-verification/pitfall_three-layer-false-green

## pitfall
- **pitfall_measuring-mainthread-stall** — 量「主緒被誰佔住」的三個地雷：探針覆蓋率、量具與被測者同緒、拿掉粗鎖會提高既有競態發生率
- **pitfall_wrapup-0163-202609090921** — 收工紀錄 TASK-0163：UCL_TaskIO 上鎖（現況沒有鎖、併發安全依賴單一主緒）—— 這是 Cmd…
