# 工作記憶索引 — tavern-read-layer-csharp

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_batch2-scope-10-to-6** — 第二批射程 10→6：task_* 三支會寫（AutoRecoverStaleLeases → AppendEvent）

## pitfall
- **pitfall_port-equivalence-byte-diff** — 移植等價性只有原樣 diff 抓得到 —— 三個差異每行單獨看都正常

## state
- **state_2026-08-21-shipped** — 已上線：兩支 service ＋ 兩個 op ＋ 後台截斷設定  ↔ identity-account-unification
