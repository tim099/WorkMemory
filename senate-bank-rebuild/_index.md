# 工作記憶索引 — senate-bank-rebuild

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_d27-coexist** — D27：新舊銀行長期並存，遷移前新銀行＝測試用（取消 C8）
- **decision_ledger-shape** — 帳本三格刻意與舊系統不同：不存 balance_after、只鎖 debit、冪等先於餘額  ↔ identity-account-unification
- **decision_single-writer-three-layers** — 單一寫入端由三層撐著，而 A2 單例鎖必須先於 A4 自動啟動  ↔ senate-backend
- **decision_transfer-atomicity-in-write-end** — 轉帳的原子性放寫入端（op=transfer ＋ 回捲），⛔ 不在頁面上拼  ↔ senate-gui-bridge/pitfall_pinned-scope-and-probe-subject
- **decision_why-rebuild-not-port** — 重做不搬：單子上的前置已過期、臨界區比寫的小、而唯一寫入端本來就存在  ↔ treasury-bank-hardening

## pitfall
- **pitfall_measurement-and-lifetime-traps** — 六隻：log 接在別人的生命週期上／自動啟動鎖住 build／量具掛住被讀成 bug／對照組名不副實

## state
- **state_bank-migration-day-2026-09-17** — 新銀行接手前要知道的五格（遷移日落地狀態）

## pointer
- **pointer_migration-readings-and-open-decisions** — 遷移範圍／具名的放棄／大小寫合併的前提，與下一步（先做常駐守衛，不是遷移）
