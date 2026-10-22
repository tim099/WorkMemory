# 工作記憶索引 — senate-repo-sync

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_git-on-cli-side** — 同步的 git 一律在 CLI 那一邊；Server 不跑 git（Tim 選 3）  ↔ senate-repo-sync/pitfall_server-git-defender

## pitfall
- **pitfall_parent-tracked-folder** — 外層已追蹤的資料夾不算巢狀 repo 的邊界（skip-worktree）
- **pitfall_server-git-defender** — 經 WMI 起的 Server 跑 git 同步會被 Defender 整顆砍掉
