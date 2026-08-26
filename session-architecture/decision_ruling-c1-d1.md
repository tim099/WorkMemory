---
id: decision_ruling-c1-d1
topic: session-architecture
title: Session 統一架構五拍板（Tim 2026-08-26）
type: decision
status: active
created_at: 2026-08-26
created_by: unknown
links: []
related_docs: []
---

①python 端不得直讀 session 資訊，全由 C# UCL_SessionService 管理。②無人在場關閉觀影場走 C-1 最小結算：已取材分鐘照算＋台帳 append，跳過收工公告；不採 C-2 掛帳標記（拒絕理由：多一種安靜的中間狀態，且無下一場則永久掛帳）。③互斥走 D-1 擋而指路：開場守衛擋下時 Cmd 回傳檔必須寫明原因（哪一場、到幾點）與處理方式（收工指令原文／等到期），祈使句、指令直接附上、不解釋代價；不採 D-2 自動關舊場（拒絕理由：把結算藏在別的動作背後、吃掉犯錯訊號、關場歸屬說不清）。④freetime.py 遷 Cmd 不留過渡 stub 直接刪 —— stub 只保護稀有呼叫者，天天用的壞了立刻現形；義務是文件同步一次到位。⑤路徑扁平化：<DataRoot>/<Kind>/sessions/<persona>.json 簡化為 <DataRoot>/sessions/<persona>.json，kind 改為 json 欄位 —— 一人一檔位使互斥成為資料形狀層的不變式（守衛仍在，負責擋+指路），sessions_log.jsonl 等 per-kind 台帳不在此射程。（記錄:basecamp）
