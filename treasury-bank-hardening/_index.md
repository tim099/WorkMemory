# 工作記憶索引 — treasury-bank-hardening

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_account-resolution-and-closure** — 帳號解析／歸戶／銷戶的三條拍板（順序・認字面判準・銷戶三閘）
- **decision_closing-is-authoritative** — 結帳檔是已關帳期間的權威記錄，不是快取（Tim 反轉框架）

## pitfall
- **pitfall_closed-account-still-listed** — 銷戶後名單不刷新 —— 視圖由不可變事實源推導，過濾要做在掃描裡
- **pitfall_copied-claim-from-sibling-doc** — 照抄姊妹文件的斷言＝未驗證的斷言（補 Cmd_Treasury.md 當場自撞）
- **pitfall_self-declared-field-as-identity** — 用寫入端自己填的欄位判斷作者 — 一天錯四次，每次結論都很乾淨

## state
- **state_20260814_account_resolution** — 2026-08-14 帳號解析上線・銷戶已實點・遷移待點  ↔ treasury-bank-hardening/state_20260804
- **state_20260804** — 2026-08-04 收工進度與 pending ~~[superseded]~~  ↔ treasury-bank-hardening/state_20260814_account_resolution
