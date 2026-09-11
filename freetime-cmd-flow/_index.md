# 工作記憶索引 — freetime-cmd-flow

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_chess-match-and-session-free-2026-09-11** — 下棋四格拍板（自動配對／不綁場次／只在輪到自己置頂／看得見有人在等）—— 而四格裡兩格是零改動  ↔ reading-library-cmd/pitfall_bytewise-diff-not-axis-list

## pitfall
- **pitfall_failsoft-warning-nobody-reads** — fail-soft 只寫進 log ＝ 沒有人看見 —— Chess 優先層因 JSON null 靜默死了很久（Str 漏第三格）
- **pitfall_negative-claims-need-evidence** — 「完全未開始」是一個我從沒查過的斷言 —— 否定句也是宣稱，也要有讀數  ↔ state_handoff-to-gura-20260818-v2
- **pitfall_voucher-wrapup-reads-expired-batch** — 收工晚於券到期 1 分鐘 ⇒ 查無被算成用完（2026-09-10 結案：14 則完全分離，我差 527ms）  ↔ freetime-cmd-flow/pitfall_voucher-expire-printed-as-used
- **pitfall_voucher-expire-printed-as-used** — 收工那一行把「到期作廢」印成「全數用畢」—— 而且只有部分人中（2026-09-10 四人 2 對 2） ~~[superseded]~~  ↔ freetime-cmd-flow/pitfall_voucher-wrapup-reads-expired-batch

## state
- **state_gura-eod-20260818** — gura 收工狀態：11 項全完成 ＋ 5 項未完 ＋ 兩條手勢
- **state_handoff-to-gura-20260818-v2** — 自由時間 Cmd 流程交接 v2（更正：AdminPage 早已存在，不要重建）  ↔ freetime-cmd-flow/state_handoff-to-gura-20260818
- **state_handoff-to-gura-20260818** — 自由時間 Cmd 流程交接（basecamp → gura）：已驗清單 ＋ 三項未完成 ＋ 三隻同族血證 ~~[superseded]~~  ↔ freetime-cmd-flow/state_handoff-to-gura-20260818-v2

## pointer
- **pointer_where-to-read** — 這條線要讀哪些檔（程式／活動 md／流程規範／跨語言讀取端）  ↔ state_handoff-to-gura-20260818
