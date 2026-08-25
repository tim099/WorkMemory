---
id: state_relay-and-window-2026-08-25
topic: streamwatch-cmd
title: 接力＋窗口＋收工三批落地（實跑全綠）；剩兩個 known-issue 待拍板
type: state
status: active
created_at: 2026-08-26
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/StreamWatch_Cmd_Reference.md, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/StreamWatch/Cmd_StreamWatch.cs]
---

2026-08-25 晚間三批全部落地並實跑驗收（ep06/08/09 三場）：

**已 ship（sha：950660cb / a400aff1；window 批在 e1b0888c）**
- 取材窗口三合成：可播放前緣（水位 or now−watch_live_guard_sec）＋進度檔位（watch_pacing_tiers List，落後量選檔）＋重疊（watch_window_overlap_sec）；不足等水位（watch_water_wait_max_sec）。
- 接力：共享前緣 `StreamWatch/relay/<primary>.json` 綁 primary session id；「先佔段再取材」；等待中每 tick 重讀前緣（兩人同等水位後醒者自動跳段）；佔段前重讀防前緣回退。個人 cursor 退居備援；`watch_relay_enabled` 可關。
- 單一主觀影者硬守衛（step=start 掃 sessions）；primary 職責＝場次設定＋熱點標記，取材全員平等；join 過期殘留自動收（gura）。
- 收工＝牆鐘 ≥ ends_at **且** 前緣 ≥ ends_at（尾段自動加班、窗口夾停 ends_at）；中斷立即結算。
- observe 貼文後印「貼文當下最新同場訊息」（關掉取材→貼文的盲區）；熱點跨場清零（step=start）。
- Page：觀影節奏區塊＝WatchPacingDraft 交給 DrawObjectData（config 加欄位繪製零改動）；resolution 改 PopupSearchCache。

**實跑讀數（驗收證據）**
- 接力鏈 ep09：me(31→44)→Sirius(41→53)→me(51→13:54)，交接各 3s、零重段；健康指標＝「前緣落後即時 Ns」。
- 加班補尾段首演：牆鐘 23:41:25 過 ends_at 23:40、前緣 23:39:30 → 印 ⏳ 繼續取材、窗口精確夾停 23:40:00、下輪「到期（實錄已補到 ends_at）」收工。

**known-issue（要 Tim 拍板再動）**
- StepStart/StepJoin 收過期殘留不補結算 —— 該場酬勞蒸發且與正常收工同形；修＝收掉前跑 SettleAsync（動計費語意）。
- 三人同場開場首評 ~6 分：Watcher 串行 ~2 分＋agent 撰寫 ~3 分；結構性（Watcher 並行/開場預熱）。
- montage sidecar 的酒館段時序已查證：在合成/OCR 之後抓（等待之後），無需再動。
