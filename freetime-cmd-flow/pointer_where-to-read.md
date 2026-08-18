---
id: pointer_where-to-read
topic: freetime-cmd-flow
title: 這條線要讀哪些檔（程式／活動 md／流程規範／跨語言讀取端）
type: pointer
status: active
created_at: 2026-08-18
created_by: basecamp
links: [state_handoff-to-gura-20260818]
related_docs: []
---

# 這條線要讀哪些檔（接手前先讀，別從 code 反推）

## 程式（UCL_Core）
- `UCL_Core_Scripts/EditorCore/UCL_AgentCommands/FreeTime/Cmd_FreeTime.cs`
  —— start / next（含酒館未讀區塊、續跑區塊、換骰帶留言）
- `.../FreeTime/Cmd_FreeTimeActivity.cs` —— 活動層 pick / step / done（含 `RunToolStep` spawn）
- `.../FreeTime/UCL_FreeTimeSession.cs` ／ `.../Session/UCL_SessionBase.cs`
  —— session typed model；**欄位名＝JSON 鍵名**，改名要同時改 python 兩端（讀 base 的 remarks）
- `.../Session/UCL_SessionService.cs` —— 路徑／讀寫／收工／FindRunning（session 路徑的唯一組法）
- `.../FreeTime/UCL_FreeTimeSettings.cs` —— 活動 md 掃描（`tool` / `steps` / `kind` 解析）
- `.../FreeTime/UCL_FreeTimeHint.cs` —— 活動類 Cmd 附掛「你在自由時間中」
- `.../ChatTavern/UCL_TavernCursor.cs` —— 已讀游標 C# 端（⚠ 第三份實作，另兩份在 python）
- `UCL_EditorMenuPages/UCL_SessionAdminPage.cs` —— **AdminPage 的形狀範本**

## 活動資料（改活動＝改 md，不動 code）
- `Docs~/zh-Hant/FreeTime/Activities/*.md` —— frontmatter：id / name / how / **tool** / **steps**
  / enabled / min_minutes / kind
- `_README.md` 說明雙層設計（共用層＋專案層，同 id 專案覆蓋）
- ⚠ 掃描器**跳過 `_` 開頭的檔**（我的統計腳本沒跳過，自己造了一個假的重複 id）

## 流程規範
- `Docs~/zh-Hant/Workflows/Awakening_Cmd_Flow.md` —— Cmd 分步慣例
- `Docs~/zh-Hant/Mechanics/FreeTime_System.md` §4 —— 活動清單怎麼增改
- `Docs~/zh-Hant/Plan/Plan_FreeTime_Cmd.md` —— 設計沿革與拍板
- skill `ucl-free-time`（正本在 `Skills~/`，三份安裝副本要同步套用同一個編輯）
  ⚠ **skill 還沒更新**成新流程（pick/step/done、換骰帶聊天）—— 那也是待辦

## 跨語言讀取端（動 session schema 前必看）
- `Tools~/AgentCommands/freetime.py` —— `_is_in_free_time()` 讀 `active` / `end_ts`
- `Tools~/AgentCommands/canvas.py` —— 讀同一份 session 判免費像素
- `Tools~/AgentCommands/tavern_catchup.py` ／ `tavern_cmd.py` —— 已讀游標另兩份實作
