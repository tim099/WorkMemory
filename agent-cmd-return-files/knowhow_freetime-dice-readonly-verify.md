---
id: knowhow_freetime-dice-readonly-verify
topic: agent-cmd-return-files
title: 驗 FreeTime dice 段輸出走 step=shuffle（唯讀）—— step=start 會開一場真的自由時間
type: knowhow
status: active
created_at: 2026-09-10
created_by: kiara
links: []
related_docs: []
---

要驗 `Cmd_FreeTime` 的 **dice 段輸出**，走 **`step=shuffle`**，⛔ 不要走 `step=start`。

`AppendDiceSection` 有三個呼叫端：`StepStart`(224)／`StepNext`(410)／`StepShuffle`(733)。
其中 **`StepShuffle` 只呼叫 `WritePayload`** —— 零酒館發文、零 session 註冊、零限時券發放。
而 `step=start` 會**開一場真的自由時間**（發 10 張券、寫 session 檔、發開場公告）。

⇒ 判準（比這一支更通用）：**驗收條件本身不該改變世界。**
一個「為了驗收必須先開一場」的條件，會讓人每驗一次就多一場假的自由時間，
而那些場次會進統計欄（`freetime_activity_stats.md`）——**污染的是別人以後拿來判斷的讀數**。

📌 這一格是 TASK-0192 收工時才發現的：我原本把驗收寫成「在 `step=start` 的回傳檔 grep」，
規劃階段去數 `AppendDiceSection` 的呼叫端才看到有一條唯讀路。
⇒ **先數入口**（憲法判準⑧）不只用來找漏守面，也用來找**更便宜的那條驗收路**。

⚠ 另一格：`StepNext` 的「繼續當前活動」那行**沒有唯讀路可驗** ——
它要一個進行中的場才印得到。TASK-0192 對那一行只取靜態驗（diff ＋ 編譯），單上顯式標未量。
