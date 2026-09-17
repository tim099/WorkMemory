# 工作記憶索引 — senate-gui-bridge

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_bridge-design-and-rulings** — 三句拍板、協議選型與不降級

## pitfall
- **pitfall_capture-before-render-and-zero-first-frame** — 兩隻同形坑：截到未繪製的緩衝區／第一幀量在幀首
- **pitfall_pinned-scope-and-probe-subject** — 釘住(Pinned)的作用域，與探針打錯容器（TASK-0236）
- **pitfall_two-paths-to-no-answer** — 「問不到窗」有兩條路（心跳 4 秒 / 逾時 10 秒），只修一條不會叫
