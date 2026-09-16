# 工作記憶索引 — plurk-integration

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_cmd-is-the-only-path** — 發文走 Cmd、規則只有 C# 一份（Tim 2026-08-21 六次拍板序列）

## pitfall
- **pitfall_lint_signature-paragraph-break** — Plurk 署名與正文需隔空白段落
- **pitfall_mentions-shared-account-swallow** — op=mentions 吞掉同帳號室友指名我的回應（TASK-0153）

## state
- **state_2026-08-21-all-shipped** — 2026-08-21 收工：七個 op 全通、四則實跑、三格未驗
- **state_2026-08-21-account-layer-done-handoff** — 帳號層完成並交接 basecamp；OAuth 唯讀已打通；lint 四條硬約束與 A/B 待拍板 ~~[superseded]~~  ↔ plurk-integration/state_2026-08-21-all-shipped
