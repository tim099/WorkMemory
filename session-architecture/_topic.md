---
id: session-architecture
title: Session 統一架構（AdminPage 下拉／單一路徑／close handler／互斥／晚安自動關／python 退場）
status: archived
created_at: 2026-08-26
related_topics: []
key_docs: [<SCP_Core>/Docs~/Coding_Standards.md, <Senate>/Docs/Workflows/SCP_Cmd_System.md, <Senate>/Docs/Workflows/Setup_And_Build.md, <Senate>/Docs/Architecture/Data_Layout.md]
task_indices: [50, 51, 52, 53, 54, 55, 56, 57, 58, 71, 127]
archived_at: 2026-09-05T15:33:34Z
archived_commit: ce38ae63d9954959d2e551ede8fde1fced95891d
---

Session 統一架構（AdminPage 下拉／單一路徑／close handler／互斥／晚安自動關／python 退場）

## 這幾份文件各自回答什麼（key_docs 的讀法）

| 文件 | 這個主題會用到的那幾節 |
|---|---|
| `<SCP_Core>/Docs~/Coding_Standards.md` | §4 路徑單一落點（含「決定點包含值存在哪」與「讀取端不准讀原始值」）／§4.5 驗收要在 exe 上／§4.6 動 Senate 的碼要停 Server／§4.7 SCP_Core 有多份工作副本 |
| `<Senate>/Docs/Workflows/SCP_Cmd_System.md` | `senate cmd sessions` 這一支住在哪、⤷Unity／⤷Server 的執行位置語意 |
| `<Senate>/Docs/Workflows/Setup_And_Build.md` | `build.sh` 的四格出廠驗收（改完 session 層要跑它，不是跑 `dotnet run`） |
| `<Senate>/Docs/Architecture/Data_Layout.md` | `sessions/<persona>.json` 的資料根與版面 |

⚠ **「新增一個 kind」的 SOP 沒有文件**（2026-09-05 查證）——
它只在 `pointer_port-0127-after-onecut` 這份記憶裡。
⇒ 那是**主題歸檔前要補的洞**：記憶會歸檔，而 SOP 是「怎麼用」，本來就該落在文件那一側。
