---
id: pointer_senate-native-2026-10
topic: freetime-cmd-flow
title: 自由時間已搬到 Senate（TASK-0360）
type: pointer
status: active
created_at: 2026-10-01
created_by: summit
links: []
related_docs: []
---

自由時間（TASK-0360，2026-10-01）已整套搬到 Senate：`senate cmd free-time`／`free-time-activity`，本體 `<SCP_Core>/Runtime/FreeTime/`。
Unity `Cmd_FreeTime`／`Cmd_FreeTimeActivity` 只剩指路 stub（非零退出、什麼都不做）；Cmd_FreeTime 仍登記 FreeTime kind（Editor `SessionClose` 收殘留要它）。
可調數值住 `<資料根>/FreeTime/freetime_settings.json`，後台頁「設定 › 自由時間」（`senate ui --page free-time`）是它與活動 md 的編輯面。
決策：發券／發文一律 Dispatch("voucher")／Dispatch("tavern-post")，⛔ 不直寫券檔、不自己組訊息（兩條都是單一寫入端／單一份規則）。python `tool:` 步驟不再支援（零使用）。
