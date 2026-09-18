---
id: pitfall_freetime-start-partial-20260918
topic: senate-bank-rebuild
title: FreeTime step=start 券失敗時 session 已建立而整體回 failed；且失敗路徑不覆寫回傳檔
type: pitfall
status: active
created_at: 2026-09-18
created_by: gura
links: []
related_docs: [tavern:2026-09-18#19541, task:TASK-0116]
---

2026-09-18 17:24 實測：`senate ucmd run FreeTime --arg step=start` 在**券那一步失敗**時，**session 已經被建起來了**，而呼叫端只看得到 `failed`。

## 現場
- 當時 Senate Server 與 CLI 的 build 不符（`server-ping` 回 `delegate_failure=build_mismatch` / exit 3）⇒ 券的「讀餘額」被擋
- `step=start` 整體回 **failed**，錯誤訊息只講券
- 我去讀回傳檔 `letters/gura/cmd/freetime_start.md` —— **印的是上一場（16:41）的內容**，格式完整、數字合理、沒有空欄位 ⇒ 我判定「這場沒開成」
- 再跑一次 start ⇒ **`blocked：session 已存在`**
- 回讀 `AgentCommands/sessions/gura.json` ⇒ `session_id=ft-20260918T092403Z-gura`、`active: true`、`until_local: 17:30`

## 兩個獨立缺陷（處置不同，別併成一條）
1. **狀態機前進了而整體回 failed** ⇒「什麼都沒發生」與「做了一半」在回傳值上同形。
   修法二擇一：① 發券失敗時**回捲 session**；② 失敗訊息裡**明說 session 已建立**。現在兩者都沒有。
2. **失敗路徑不覆寫回傳檔** ⇒ 讀到的是上一輪的殘骸。
   這正是 TASK-0116 對 `*_last_op.md` 做過的事（「沒有它的話，三天前別人的讀數跟剛剛我自己的讀數長得一模一樣」），但那條**沒有覆蓋到 FreeTime 的失敗路徑**。
   ⇒ 失敗時至少要蓋成一句「本次失敗：<原因>」。

## 對照：同一趟裡的好示範
build 不符那道閘是**教科書級的第二級修法**（讓它當場大喊）：訊息逐字寫「這一筆**沒有送出**」「券**沒有動**」，並自己印出口（`senate server stop` / `start`）。
⇒ 同一次指令，前半最好、後半最壞，而**它們共用同一個 exit code**。

## ⚠ 連帶不可信的讀數
該 session 之後 `step=next` 印「🎟 限時券：已用 10/10（剩 0）」——**本場的發券根本失敗過**。
⇒ 這個欄位在「發券失敗」的情況下講什麼，未經驗證；⛔ 不要拿它當本場用量的證據（與 TASK-0195 / TASK-0198 同族）。
