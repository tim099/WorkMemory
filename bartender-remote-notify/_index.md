# 工作記憶索引 — bartender-remote-notify

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_read-ack** — 已讀確認+冷卻+retry 拍板（兩軌分離）  ↔ compile-verification/decision_completion-and-freshness
- **decision_read-python-act-csharp** — 判讀在 python、操控在 C#（含三條實測改寫的規格）

## pitfall
- **pitfall_blocks-main-thread** — 通知流程卡死 Unity main thread（待用 UniTask 改造）
- **pitfall_sendinput-true-not-received** — SendInput 回 true ≠ 對方收到（Enter 無效／掉字／假回報）
- **pitfall_type-char-drop** — 逐字輸入掉字實錘（/uclding 少一槓）+ 修法方向

## state
- **state_2026-08-03-basecamp** — 現況與 pending（2026-08-03：前景驗證改版 + 通知池實測 + 主執行緒問題）  ↔ bartender-remote-notify/state_current
- **state_2026-08-03c** — 施工進度 2026-08-03c（全案 commit + 實戰閉環, 剩 char-drop 修法待排程）  ↔ bartender-remote-notify/state_2026-08-03b
- **state_2026-08-03** — 施工進度 2026-08-03（已讀機制完工待實測） ~~[superseded]~~  ↔ unitask-editor-async/knowhow_unitask-patterns, bartender-remote-notify/state_2026-08-03b
- **state_2026-08-03b** — 施工進度 2026-08-03b（OCR 守門撤除 — 點標題列無害拍板） ~~[superseded]~~  ↔ bartender-remote-notify/state_2026-08-03, bartender-remote-notify/state_2026-08-03c
- **state_current** — 現況與待辦（2026-08-02 夜） ~~[superseded]~~  ↔ commit-identity-pipeline/state_current, bartender-remote-notify/state_2026-08-03-basecamp

## pointer
- **pointer_npc-map** — 酒保 NPC 生態地圖（端酒/催促/廣播/通知 — 哪些後台可設）
