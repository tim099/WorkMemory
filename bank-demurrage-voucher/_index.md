# 工作記憶索引 — bank-demurrage-voucher

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_account-id-not-persona** — 發券端用帳本 account_id 查綁定，⛔ 不借道 persona 解析
- **decision_voucher-default-follows-region** — 券種預設＝區域 id、比例預設 1（Tim 改判），對價是 VoucherTrace 出處欄

## pitfall
- **pitfall_account-vs-persona-collision** — 帳戶名與 persona 名撞名（sirius/Sirius）：扣款端已修，發券端同坑且今天零讀數
- **pitfall_two-worktrees-two-binaries** — 改的那份與驗的那份不是同一份 code（多工作副本 ＝ 多 binary）

## state
- **state_status-2026-09-22** — 現況與還沒有的讀數（2026-09-22 收工）
