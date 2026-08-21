---
id: decision_cmd-is-the-only-path
topic: plurk-integration
title: 發文走 Cmd、規則只有 C# 一份（Tim 2026-08-21 六次拍板序列）
type: decision
status: active
created_at: 2026-08-21
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/Plurk_Posting_Workflow.md, Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/Plurk_Maintenance.md, Assets/Plugins/UCL_Core/Editor/Plurk/Cmd_Plurk.cs]
---

Plurk 發文一律走 `Cmd Plurk`（C#），**規則本體只有 C# 一份**。

拍板序列（Tim 2026-08-21，同日四次）：
① 交付單五欄（增 `公開度`），沒填就擋、⛔ 不預設「所有人」（後續 ② 改為預設所有人，見下）
② 長文拆則「自主判斷、預設走回應」—— 判準：這是一篇被切成兩半，還是兩篇？
③ 「這部分可以走 c# CMD」⇒ post 是唯一寫入端，lint 強制前置（規則長在必經路上）
④ 有個人帳號走個人、沒有就走共用（＝既有三段解析順序，**沒有加開關**）
⑤ 自決直發授權（skill 內）：預設公開度「所有人」，agent 自審通過即可自帶 confirm=1 直發
⑥ 「CMD 流程應該可以跑驗證」⇒ 補 `op=get` 唯讀回讀（歸路搬進 Cmd）

⛔ python `plurk.py` 已由 Tim 移除。它曾是唯讀診斷＋唯一歸路工具 ——
歸路現在住 `op=get`，這是它存在的理由（不是方便，是「我送出了 ≠ 它在那裡」）。
