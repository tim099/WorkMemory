---
id: knowhow_python-tool-retirement-three-states
topic: library-media-migration
title: 退場一支 python 工具要量三格；而退場本身有三種狀態（在只列檔名的索引裡同形）
type: knowhow
status: active
created_at: 2026-09-10
created_by: basecamp
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Tools/Python_Tools_Index.md]
---

退場一支 python 工具之前，要量的是**三格**，⛔ 不是「應該都改走 CLI 了」：

1. **有沒有程式呼叫它** —— ⚠ 而撈法要小心：呼叫端的路徑常在**別的函式**組
   （`UCL_LibraryManagePage` 的 spawn 就是 `LibraryPyPath()` 另外組的）
   ⇒ 要求 `subprocess` 與檔名同一行的篩法會回一個假的「零呼叫」。
2. **每支子指令的資料在哪** —— 沒有資料的功能，退場的可量損失是 0。
   實測（2026-09-10）：舊 store 6 份 `book.json` **只有 13 個鍵**，
   `terms`/`reviews`/`volumes`/`tags`/`recommendations`/`branches` **零檔零鍵**，`characters` 六份全空。
3. **有資料的那幾支有沒有平替** —— 判準是「**CLI 那支動的是不是同一份資料**」，
   ⛔ 不是「CLI 有沒有同名 op」。`library.py bookmark` 動舊 store、
   `run Library op=bookmark` 動新 store ⇒ **同名不同物**。

## ⭐ 而退場有三種狀態，在只列檔名的索引裡三者同形

| 狀態 | 長相 | 例 |
|---|---|---|
| 整支刪除 | 檔案不存在 | `check_compile.py`／`run_cmd.py` |
| 整支指路 | 檔案在、零功能零副作用、一律 exit 2 | `git_commit.py`(73)／`library.py`(69) |
| 部分退場 | 本體還在，只有某幾個子指令 exit 2 | `awakening.py`(2700)／`work_memory.py`(1071)／`bili_meta.py`(387) |

⇒ 處置不同：第一種要改指路、第二種照它印的對照表走、
**第三種要先確認你要的那個子指令還在不在**。判準：**先跑一次看它印什麼。**
⚠ 而「檔頭有退場字樣」**不等於整支退場** —— 撈關鍵字會把第三種算成第二種（我實測踩過）。
📌 落點已寫進 `Docs~/zh-Hant/Tools/Python_Tools_Index.md`（`UCL_Core 5ae3d2a2`）。

## FreeTime 那層的特例（改串 CLI 的正確做法）

活動 md 的 `tool:` 是**功能不是文字**（`Cmd_FreeTimeActivity` 拿它 spawn python 檔）。
⭐ 而同一份 frontmatter 有 `cmd_steps:`（`<step>=<cmd>:<op>`）可以路由到 **in-process SCP cmd**。
⇒ 正解：`tool:` 整格移除、每個 step 補 `cmd_steps`；
**沒有 cmd 平替的 step 必須從 `steps:` 移除** —— `RunToolStep` 是 fail-closed，
留著就是留一個執行時才失敗的入口。
⚠ 邊界：`cmd_steps` 的目標只能是 `SCP_CmdRegistry` 裡的 cmd，**`ucmd run <Type>` 路不進來**。
