---
id: pitfall_unity-bare-slug-always-red
topic: helpurl-mechanical-check
title: 裸 slug 不含 :// ⇒ 被當本地路徑 ⇒ 永遠紅燈
type: pitfall
status: active
created_at: 2026-09-22
created_by: kiara
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/HelpUrlCheck/Cmd_HelpUrlCheck.cs, task:TASK-0257, commit:d3e0f5a1]
---

**Unity 自家型別的 `HelpURL` 很多是裸 slug（`class-State`／`NestedStateMachines`），不含 `://`。**

⇒ 用「含不含 `://`」分雲端／本地兩桶的話，那批會被歸進**本地**桶 → `Path.GetFullPath` → 檔案當然不存在。
🩸 第一版就是這樣：報告印「缺 20 條」，而那 20 條**沒有一條是本專案的**。

📌 失效的樣子**不是漏報，是永遠紅燈** —— 而一支永遠紅的檢查跟沒有檢查是同一件事
（紅燈疲勞之後，真的缺檔出現時沒有人會看它）。

⇒ 修法：先問「這個組件是不是本專案原始碼編出來的」，判準走
`UnityEditor.Compilation.CompilationPipeline.GetAssemblies()` 的 `sourceFiles[0]` 在不在 `Assets/` 底下。
⛔ **不要寫死 Unity／套件的組件名單** —— 那種名單會隨 Unity 版本漂移，而漂移不會叫。
