# 工作記憶索引 — sculpt-town-kit

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## knowhow
- **knowhow_civic-styles-fallback** — 公共建築照地區文化挑、加勒比海與英國那一套、擺到城裡（TASK-0772）

## pitfall
- **pitfall_apply-rerun-pivot** — apply.py 改設計後重跑：關聯的 pivot 不會跟著來源縮、改色也要 overwrite、組裝件的固定點清單含子件
- **pitfall_civic-build-pitfalls** — 城鎮建築套件：layers 漏列、斜的一格厚就浮空、縮框先重下關聯、布旗不跟固定點（TASK-0772）
