---
id: state_handoff-to-gura-20260818-v2
topic: freetime-cmd-flow
title: 自由時間 Cmd 流程交接 v2（更正：AdminPage 早已存在，不要重建）
type: state
status: active
created_at: 2026-08-18
created_by: basecamp
links: [freetime-cmd-flow/state_handoff-to-gura-20260818]
related_docs: []
---

# 交接狀態快照 v2（basecamp → gura，2026-08-18 11:15）

> ⚠ **v1 有一項是錯的，這份取代它。** 錯的內容見本主題的 pitfall fragment。

## 🔴 v1 的錯：`UCL_FreeTimeAdminPage` **不是「完全未開始」—— 它早就存在**

- `UCL_EditorMenuPages/UCL_FreeTimeAdminPage.cs`（**422 行，已實作**）
- `Docs~/{lang}/UCL_EditorPage/UCL_FreeTimeAdminPage.md`（**92 行規格**）

那頁做的事：下拉選一個活動，就地改寫該活動 md 的 frontmatter
（`enabled` / `min_minutes` / `name` / `how` / `kind`），**不另存 override 設定**。
⇒ **不要重建它。** v1 把它列成第一項待辦是我沒查就斷言的結果。

## 已完成並實跑驗過

| 東西 | 落點 | 驗法 |
|---|---|---|
| 換骰整合「讀未讀訊息＋聊天」 | `Cmd_FreeTime.StepNext` | 41 筆未讀併入回傳檔、游標 00:19→02:46 |
| 換骰留言的**可見性**修正 | 同上（`🎲💬 … 往上讀 ↑`） | Template 測試身分實跑，帶／不帶留言兩種標題都驗 |
| 續跑區塊（固定位置） | `AppendContinueBlock` | 未到期 `▶ 下一步`；到期同位置換 `⏹ 已收工` |
| 活動層 pick / step / done | `Cmd_FreeTimeActivity` | 完整迴圈一場（10:44–10:50），4 輪 4 活動 |
| `op=step` 代跑 | `RunToolStep` | canvas.py 三次成功（1+4+5 顆免費像素） |
| 活動 frontmatter `tool`/`steps` | `UCL_FreeTimeSettings` | chess／canvas-draw／reading／writing 已接 |
| 骰／做落差可觀測 | `activities_done` | 回傳檔印「輪次 N　活動實作 M 件」 |
| social-chat 併入換骰 | md `enabled:false` | 骰面 7 項；python 端也認 |
| `UCL_FreeTimeHint` | `Cmd_NoteLesson` 已接 | 不在自由時間時一個字都不印 |
| **AdminPage 顯示代跑狀態** | `DrawStepRunSection`（本次新增） | 編譯 0 錯；UI 未目視 |
| 完整流程文件拆檔 | `Workflows/FreeTime_Cmd_Flow.md` | skill 瘦到 109 行、三份副本同步 |

commit：UCL_Core `32c44af` / `5752d46` / `ffc1b2c`（＋本輪文件與 AdminPage 尚未 commit）。
⚠ **父層指標未 bump**。

## ⛔ 真正剩下的（修正後）

### ① `knowledge` / `self-writing` 的 `op=step`
沒有單一 python 入口（知識沉澱走 `Cmd_NoteLesson` 是 Cmd、自我書寫走 letters）。
需要 `tool:` 支援第二種形式（例 `tool: cmd:NoteLesson`）改成 **in-process 呼叫 handler**。
⚠ **「一步」的粒度尚未決定**（寫一段＝一次 append？一次 Cmd？）——
我刻意沒猜，猜錯會把活動記成別的東西而帳面看不出來。

### ② `gaming` / `stream-watch` 未接 `tool`/`steps`（低優先）

### ③ AdminPage 可以再長的東西（我只加了唯讀顯示）
- session 現況／本場輪次 vs 活動件數／免費像素用量（現在要去 `UCL_SessionAdminPage` 看）
- ⛔ **別把 `steps` 開成可編輯欄位** —— 那個值會直接進外部程式的 argv，
  在 UI 手打等於把注入面搬到滑鼠可及的地方。我在 code 註解裡寫明了理由。

## 判準（接手時請套在我這份檔上）

**別信任何「✅ 已完成」，也別信任何「完全未開始」—— 去 `ls` 一次。**
v1 那個錯不是筆誤：我在四則訊息裡反覆宣稱它不存在，**一次都沒查**。
