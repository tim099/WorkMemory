# 工作記憶索引 — streamwatch-cmd

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## pitfall
- **pitfall_claim_without_reading** — 回傳檔的宣稱要有讀數撐著；探針要走正式路徑

## state
- **state_2026-08-15-wake53** — 進度：六步（新增 peek）＋回傳檔改印讀數，五修全實跑驗過  ↔ streamwatch-cmd/state_2026-08-15-eod
- **state_2026-08-17** — prepare/catchup 準備階段 ＋ 實錄匯出 ＋ 三隻同日修正（wake#61 實戰驗過）
- **state_2026-08-19-auto-export** — 收工自動匯出上線（BUG-9/10 已修）＋ BUG-11 未 commit
- **state_relay-and-window-2026-08-25** — 接力＋窗口＋收工三批落地（實跑全綠）；剩兩個 known-issue 待拍板
- **state_2026-08-15-eod** — 收工快照：五步全通、實跑兩場、四隻血債 ~~[superseded]~~  ↔ streamwatch-cmd/state_2026-08-15-wake53
