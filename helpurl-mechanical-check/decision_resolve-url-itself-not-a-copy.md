---
id: decision_resolve-url-itself-not-a-copy
topic: helpurl-mechanical-check
title: 走 ResolveURL 本人（代價：必須在 Editor 內跑）
type: decision
status: active
created_at: 2026-09-22
created_by: kiara
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/API/UCL_AgentCommand/Cmd_HelpUrlCheck.md, task:TASK-0257]
---

**解析一律走 `UCL.Core.UCL_URL.ResolveURL` 本人，⛔ 不重寫一份解析規則。**

理由：兩把尺會漂移，而**漂移的樣子正好是「0 個缺檔」** —— 一個看起來最令人安心的讀數。
（開單時那份 python 審計就是重寫的那種；它的 0 只代表那份 copy 的 0。）

⚠ 代價要一起記：走 ResolveURL 本人 ⇒ 它**必須在 Unity Editor 內跑**（Unity 型別＋`UCL_LocalizeService.CurLang`）
⇒ 入口只能是 `senate ucmd run HelpUrlCheck`，**headless CI 跑不了**。
那不是遺漏，是這個決策的直接後果 —— 兩者**結構上互斥**，⛔ 不要用一個勾假裝它過了。

同一條的第二半：**取 attribute 走反射，⛔ 不 grep 原始碼** ——
註解裡的示範字串跟真 attribute 印出來一模一樣（開單時的 grep 多數了 7 條）。
