---
id: pointer_port-0127-after-onecut
topic: session-architecture
title: TASK-0127 ⑦ 之後的落點：session 層只剩 SCP_Core 一份（＋新增 kind 的 SOP、兩個地雷）
type: pointer
status: active
created_at: 2026-09-04
created_by: basecamp
links: [session-architecture/pointer_port-0127-entry-points]
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

## 要新增一種 session kind 的時候（⑩ 的文件還沒寫，先記在這）

1. `SCP_ActivitySessionKind.Kinds` 加一筆 —— **門檻不變：先跑一場真的再加進來。**
   （沒跑就加＝多一格「看起來查過了」的假讀數：欄位缺席時 typed model 只會拿到預設值，
   而 `active=false` 跟「沒這場」長得一樣。）
2. 專屬欄位 ⇒ 開一個 `: SCP_ActivitySession` 的子類別，走 `Load<T>`。
   ⛔ 不要寫 `SerializeToJson` 之類的 override —— `SCP_JsonMapper` 寫原生 bool、
   `[SCP_Ignore]` 排除欄位，別套別的框架的直覺（UCL 那套只看 `[UCL_HideInJson]`，
   `[NonSerialized]` 它不看 —— 09-04 上午實測真的把整包 `RawJson` 寫進檔）。
3. 要結算 ⇒ 實作 `SCP_IActivitySessionCloseGateway` 並在宿主注入 `GatewayHost.Factory`。
   ⚠ 介面是 `TryClose`（**整步關場**）不是 `TrySettle`：對面靠 `active=true` 判斷該不該結算，
   先關場再委派 ⇒ 結算永遠不發生而兩邊都不報錯。
4. 加一格 selftest —— 而且要有**反向對照**：不只驗「子類別寫得出讀得回」，
   要驗「**讀成基底寫回去之後專屬欄位還在**」。只驗前者的話，一個基底寫回就吃鍵的實作也會全綠。

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
