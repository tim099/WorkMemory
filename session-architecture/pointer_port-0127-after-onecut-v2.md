---
id: pointer_port-0127-after-onecut-v2
topic: session-architecture
title: TASK-0127 ⑦ 之後的落點：session 層只剩 SCP_Core 一份（＋兩個地雷；SOP 已搬進文件）
type: pointer
status: active
created_at: 2026-09-05
created_by: basecamp
links: [session-architecture/pointer_port-0127-after-onecut]
related_docs: []
---

⑦ 一刀切之後的落點（2026-09-04 傍晚，basecamp wake#89）。取代 supersede 掉的那份 ——
那份寫「兩邊都在，只有 UCL 那份被呼叫」，**現在不成立了：UCL 那側的 session 層已經不存在。**

## 唯一的 session 層（就是這一份，沒有第二份）

- `SCP_Core/Runtime/Session/SCP_ActivitySession.cs` —— 模型。**可被繼承**（2a18546 拿掉 sealed）
  · `Raw` 仍然要留：讀成子類別 ≠ 認識全部的鍵（基底路徑一個 kind 專屬欄位都不認識）
- `SCP_ActivitySessionStore.cs` —— 路徑／`Load`／`Load<T>`／`Save`／`TryStart`／`Close`／
  `CloseWithSettlement`／`LoadAll`／`ListPersonas`
- `SCP_ActivitySessionKind.cs` —— kinds 登記表／`SCP_IActivitySessionCloseGateway`／`GatewayHost`
- `SCP_Cmd_Sessions.cs`（`senate cmd sessions`）／`SCP_GuiSessionAdminPage.cs`（`senate ui --page sessions`）
- `Senate/src/Senate.Core/SenateSessionCloseGateway.cs` —— ⏳ 過渡件（退場條件 TASK-0106）

## Unity 那側現在剩什麼

- `Session/Cmd_SessionClose.cs` —— **委派目標，要留**（結算在 Editor）
- `Session/Cmd_SessionStatus.cs` —— 唯讀，留給 agent
- typed 子類別兩個：`UCL_FreeTimeSession`（3 欄）／`UCL_StreamWatchSession`（~30 欄）
  ⇒ 兩個都 `: SCP.Core.Session.SCP_ActivitySession`
- `UCL_AgentCommandsPath.ScpDataRoot` —— 資料根的 typed 轉接（**不是** session 專屬入口，任何 SCP 的 IO 都走它）
- ⛔ 已刪：`UCL_SessionService`／`UCL_SessionBase`／`UCL_SessionKind`／`UCL_SessionAdminPage`
  ＋ ToolEntry ＋ 四語 localize 兩鍵

## 要新增一種 session kind 的時候 ⇒ **走文件，不走這裡**

📄 **`<SCP_Core>/Docs~/Session_Kinds.md`**（2026-09-05 落地）——
登記名字／開場走 `TryStart`／宿主登記行為／收工兩條路不互叫／交付前五格讀數，全在那裡。

🩸 **這一節本來寫在這裡，而那是錯的落點**：記憶會歸檔（主 Task 收尾那天），
而「怎麼用」跟著鷹架一起消失。⇒ 已整段搬進文件，**這裡只留指路**。
📌 判準：**記憶回答「為什麼／怎麼踩過」，文件回答「怎麼用」** —— 兩邊都寫就是漂移。

## ⚠ 兩個地雷（都咬過我，各一次）

1. **`Assets/` 底下的 csproj 不要 `dotnet build`**：`Assets/Plugins/SCP_Core/SCP_Core.csproj` build 出來的
   `bin/` `obj/` 就長在 Unity 會 import 的位置 ⇒ 同名 assembly 被匯入兩次，
   `CS1704` **報在 `UCL_Core` 這個完全無關的 assembly 上**。要編就編 Senate 那份工作副本。
   （同一個 repo 掛兩份，而只有一份會被 Unity import。）
2. **`publish/senate.exe` 的 build 時間要先確認**：selftest 格數是 32 還是 31 取決於那顆 exe 幾點 build 的。
   而 `build.sh` 會收掉常駐 Server 與 GUI 視窗 ⇒ Tim 開著視窗時先問再 build。

## ⊘ ⑧ 沒有 migration 可設計

舊 `UCL_SessionService.SessionPath` 與新 `SCP_ActivitySessionStore.PathOf` 指同一個檔
（`<DataRoot>/sessions/<persona>.json`，逐段相同）⇒ 沒有「新家」，沒有並存的那一刻。
⑧ 是 `⊘ 不適用`（缺真值），不是 `[ ] 未完`（缺讀者）。

## 檔案格式有一個看得見的差異（不是 bug，但要說得出來）

寫入端換成 `SCP_JsonWriter` ⇒ `"rounds":2` 變成 `"rounds": 2`（冒號後多一個空格）。
鍵與值零差異；`json.loads` 不受影響（session 的 python 讀取端 2026-08-26 已退役）。
⚠ 任何**拿文字比對** session 檔的東西會看到差異。
