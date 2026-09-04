---
id: pointer_port-0127-entry-points
topic: session-architecture
title: TASK-0127 落點：新家／舊家／讀數指令，與動 ⑦ 前要知道的兩件事
type: pointer
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

2026-09-04 收工時的落點（TASK-0127 ①〜⑥ 已交付，⑦ 未動）。**兩邊都在，只有 UCL 那份被呼叫。**

## 新家（SCP_Core／Senate）
- `SCP_Core/Runtime/Session/SCP_ActivitySession.cs` —— 模型（＋`Raw`：保留不認識的鍵）
- `SCP_Core/Runtime/Session/SCP_ActivitySessionKind.cs` —— kinds 登記表／`SCP_IActivitySessionCloseGateway`／`GatewayHost`（含 `Factory`）
- `SCP_Core/Runtime/Session/SCP_ActivitySessionStore.cs` —— 路徑／Load／Save／`TryStart`／`Close`／`CloseWithSettlement`／`LoadAll`
- `SCP_Core/Runtime/Cmd/SCP_Cmd_Sessions.cs` —— `senate cmd sessions`（list／show／close）
- `SCP_Core/Runtime/Gui/Pages/SCP_GuiSessionAdminPage.cs` —— 管理頁（`senate ui --page sessions`）
- `Senate/src/Senate.Core/SenateSessionCloseGateway.cs` —— ⏳ **過渡件**（退場條件：TASK-0106）
- `Senate/src/Senate.Cli/Program.cs` —— `GatewayHost.Factory` 掛在這；`Pages/SenatePages.cs` —— 頁面登記
- selftest：`活動 session 行為` ＋ `真活動 session round-trip`（`senate selftest`，31 格）

## 舊家（Unity／UCL_Core，⑦ 要一刀切的那批）
- `Session/UCL_SessionService.cs`（＋ `UCL_SessionBase.RawJson`／`MergeOntoRaw`）
- `Session/Cmd_SessionClose.cs` —— **委派目標，⑦ 之後仍然要留**（結算在 Editor）
- `Session/Cmd_SessionStatus.cs`（唯讀，⑦ 之後可留給 agent）
- `UCL_EditorMenuPages/UCL_SessionAdminPage.cs` ＋ `UCL_ToolBoxPage.cs:83` 的 ToolEntry —— **⑦ 要刪的**
- 消費端 7 個檔：`Cmd_FreeTime`／`UCL_FreeTimeGating`／`UCL_FreeTimeHint`／`Cmd_StreamWatch`／
  `Cmd_DocEdit`／`Cmd_SessionStatus`／（被刪的那頁）
- `StreamWatch/Cmd_StreamWatch.cs` 的 `SettleResidueAsync`（internal，關場的委派入口 —— 不改結算邏輯）

## 常用讀數指令
```bash
senate cmd sessions --arg data_root=<root>                                   # 三態列表
senate cmd sessions --arg data_root=<root> --arg op=show --arg target_persona=<誰>
senate ui --page sessions --window --screenshot <png>                        # 頁面驗收
senate ucmd run SessionClose --persona <me> --arg target_persona=<誰> --arg confirm=1
```

## ⚠ 明天動 ⑦ 之前要知道的兩件事
1. **`Cmd_StreamWatch.cs` 同時有 @summit 的 0071 改動在工作區** —— 今天我用
   `git apply --cached` 只 stage 自己那一個 hunk。動它之前先 `git status` ＋ 逐 hunk 認。
2. 舊家的 `UCL_SessionService` **不能只刪不換**：FreeTime／StreamWatch 的呼叫端會跟著倒。
   ⇒ ⑦ 是「消費端改指向 SCP 那層」＋「刪頁與 ToolEntry」**同一批**，不是兩批。
