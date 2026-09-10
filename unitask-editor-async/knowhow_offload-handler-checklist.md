---
id: knowhow_offload-handler-checklist
topic: unitask-editor-async
title: 把 Cmd handler 移出主緒的三問 ＋ EnterBackground 必須帶 args
type: knowhow
status: active
created_at: 2026-09-10
created_by: summit
links: []
related_docs: []
---

把一支 Cmd handler 移出主緒之前要問的三件事（`Cmd_Task` 2026-09-10 實作，`97328dd3`）：

① **它慢在哪** —— 逐 op 攤開 `_cmd_slow.jsonl`。`Cmd_Task` 的讀數：**不發公告的 op 也是秒級**
（`wrapup` 4185ms／`check` 3640ms／`update` 2564ms）⇒ 慢的是 handler 內的檔案 IO，不是嵌套的酒館那一跳。
⚠ Runner 側 `phases[]` 加總只有約 10ms ⇒ **phases 量不到 handler 裡面**，別拿它當「慢在哪」的答案。

② **主緒 only 的 API 掃過了嗎** —— grep `AssetDatabase` / `PlayerPrefs` / `EditorPrefs` /
`Application.dataPath` / `EditorUtility` / `EditorApplication`。Task 族**全 0 命中**；
唯一碰 Unity 的是三族**有快取**的路徑解析器，而那正是 `EnterBackground()` 先摸一次的東西。

③ **有沒有嵌套呼叫別的 Cmd** —— `UCL_TaskNotify` 是 `new Cmd_Tavern(); await ExecuteAsync(...)`，
而 `Cmd_Tavern` 自己第一行就 `EnterBackground(args)`。**從背景緒呼叫它 ⇒ prewarm 靜默失效**
（路徑 getter 丟例外被吞）。⇒ 通知前 `await UniTask.SwitchToMainThread()`，讓子 Cmd 自己按設計切。
⛔ 不在 Notify 裡幫它切（那會變成第二套規則）。

🩸 **`EnterBackground` 一定要帶 `args`** —— `offloaded` / `bg_tid` 是從 args 的 `_cmd_id` 戳出去的。
不帶就「切了但不記錄」，而 `offloaded=false` 同時是「沒 offload」與「忘了帶 args」⇒ 我照它的用法註解
打了無參數版，拿到一個假紅燈（既有 6 個呼叫點全都帶）。註解已修（同筆 commit）。

✅ 驗收的正確形狀：`offloaded=True` ＋ `bg_tid ≠ main_tid` ＋ **`elapsed_ms` 幾乎不變**
（工作沒變少，只是換緒）。⛔ **不要用斷拍歸因當對拍** —— 見下一筆。
