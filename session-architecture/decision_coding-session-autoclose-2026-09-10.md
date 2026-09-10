---
id: decision_coding-session-autoclose-2026-09-10
topic: session-architecture
title: 施工場綁單＋進 in_review 自動收場（目的是縮短持有）；expect_files 改必填
type: decision
status: active
created_at: 2026-09-10
created_by: gura
links: []
related_docs: []
---

Tim 2026-09-10 拍板（TASK-0193）：Coding 場可綁**多張單**，綁定單**全部進 `in_review` 或 done** 就自動收場；也可以手動收。**目的是縮短持有**，避免同時在改 Unity 端 C#（**Senate 那邊可以同步改**）。

實作要點：
- 判準是 **`in_review` 不是 `done`** —— 不等全部驗完。`in_review` 被退回 ⇒ **下次動工開新的場**，不把舊場接回來。
- 掛在 `senate cmd commit` **推單之後** ⇒ 不必記得收場（同 `Fixes` 掛在 commit 上的理由）。
- 五種「不收」各自說得出理由：`no-session`／`unbound`／`task-missing`（**查無 ≠ 做完**）／`still-working`／**`compile-red`（紅燈不收，場還是你的）**。
- 射程只到 **Unity 端 C#**（含 `Assets/` 底下的 submodule）。

🩸 **我加的一條守衛被目的推翻了**：原本寫「工作區還有未提交的 `.cs` ⇒ 不收」，那會讓場**握得更久**，跟本單目的相反。改成**照收但把清單記進 `left_dirty_cs` 並出聲** —— **那是資訊不是閘**。
⇒ 一般形：**一條守衛的對錯取決於它擋住的是什麼代價。** 我算了「別人搶進來改同一批檔」，沒算「場一直不放，別人根本進不來」。

⚠ 而 `senate cmd commit` 的「pre-staged 硬擋」**不可實作**：`Cmd_AutoCommit` 自己 stage 所以外來的認得出來；commit **靠呼叫端 stage** ⇒ 無從分辨「非本次要提交」。改成 **`expect_files` 必填** ＋ `any` 顯式放棄留痕。血證：`expect_files=2` 擋下「實際 staged 5」，多的三支是同事正在寫的檔 —— **而救了那一筆的旗標當時是選填的。**
