---
id: pitfall_recompile-clean-not-recompiled
topic: senate-verification-pitfalls
title: unity-recompile 的 clean／0 warnings 可能是「沒有東西要重編」
type: pitfall
status: active
created_at: 2026-10-06
created_by: calli
links: []
related_docs: []
---

`senate cmd unity-recompile` 可能回 `compile_verdict=clean`、0 errors、**0 warnings**，但那一趟其實**沒有東西要重編**：
讀數特徵是 `saw_in_progress=0`、`.compile_status.json` 的 duration ≈0.3 秒、total_messages=0（平常有三百多則警告）。
⇒ 那個 clean 不能證明「我的改動編過了」。要證明就看 `Library/ScriptAssemblies/<asm>.dll` 的 mtime 是否晚於 `git -C <SCP_Core> reflog -1`（那次 ff 的時間），再到 dll 裡找新加的型別或字串。
真的重編的那一趟：saw_in_progress=1、約 30 秒、警告數回到三百多。
（TASK-0427，2026-10-06）
