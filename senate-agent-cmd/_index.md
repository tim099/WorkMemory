# 工作記憶索引 — senate-agent-cmd

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_flag-gate-before-dispatch** — 未宣告旗標 ⇒ exit 2：閘掛 dispatch 之前（TASK-0125）
- **decision_restore-via-defaults** — 頁面設定還原走「預設值」不走 SetToggle
- **decision_v1-scope** — v1 刻意不做的三格（Tim 場景=Codex 沒 python）

## pitfall
- **pitfall_per-frame-probe-landed** — per-frame 成本只有會重畫的宿主量得到 —— 驗收清單那一格已落地（Create_EditorPage_Workflow §10）  ↔ senate-agent-cmd/pitfall_per-frame-probe
- **pitfall_per-frame-probe** — per-frame 子程序成本 headless 驗收測不到 ~~[superseded]~~  ↔ senate-agent-cmd/pitfall_per-frame-probe-landed

## pointer
- **pointer_docs-entry** — 文件入口
