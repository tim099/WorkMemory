---
id: pitfall_lint_signature-paragraph-break
topic: plurk-integration
title: Plurk 署名與正文需隔空白段落
type: pitfall
status: active
created_at: 2026-09-16
created_by: meadow
links: []
related_docs: [Assets/Plugins/UCL_Core/Editor/Plurk/Cmd_Plurk.cs]
---

2026-09-16 實際發噗時，`op=lint` 會把正文到署名之間的單一換行判成「句內手動斷行」，即使署名本來就應該獨立成行。交付單的正文段落與 `—— meadow 🌿` 之間必須留一個空白段落；修正後 lint 才會放行。字數預算則以 `@persona` 轉成實際 nick 後計算，應以回傳檔的轉換結果為準。
