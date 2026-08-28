---
id: pitfall_silknet-imgui-no-modifier-keys
topic: senate-backend
title: Silk.NET ImGuiController 從來沒送 modifier ⇒ 所有 Ctrl 快捷鍵無效（打字正常）＋ keydebug 診斷基建
type: pitfall
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/knowhow_imgui-clipboard-bridge]
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

接續 knowhow_imgui-clipboard-bridge：接上 callback **還是不夠**。Tim 實測 Ctrl+V 仍然沒反應。詳見 Senate Docs/Logs/Decisions.md D19 ⑪。

**根因：Silk.NET 的 ImGuiController 從來沒送 modifier**
解析 `Silk.NET.OpenGL.Extensions.ImGui.dll`（2.23.0）的 metadata #Strings heap：
- 有 `AddKeyEvent` / `TranslateInputKeyToImGuiKey` / `AddInputCharacter`
- **沒有 `ModCtrl` / `ImGuiMod`**（只有 `get_KeyCtrl` 讀取）
- 沒有 `SetClipboardTextFn` / `GetClipboardTextFn`（確認 controller 完全不碰 clipboard ⇒ 我們的橋是唯一那條）

⇒ 它沒把 modifier 狀態送進 ImGui，而 ImGui 的快捷鍵判斷的是 `io.KeyMods` ⇒ **Ctrl+V / Ctrl+C / Ctrl+A / Ctrl+X 全部無效，而單獨打字照樣正常**（那條走 AddInputCharacter，跟 modifier 無關）。
📌 兩者症狀不同形正是它難被發現的原因 —— 「這個欄位只能手打」聽起來像功能缺失，不像 bug。

**診斷方法論（最值得記的一格）**
⭐ 把範圍切開的是 Tim 補的一句「**但是按鈕的貼上 OK**」—— 那一句立刻把「剪貼簿實作 / SCP_GuiHost / Win32 讀取」整段排除，剩下唯一嫌疑是「ImGui 收不到組合鍵」。
📌 一般形：**一個「哪一半壞了」的問題，最快的尺是找出「哪一半還好」。**

🩸 我第一次掃 dll 用「抓所有 ASCII 序列」的土法，連 `ImGuiKey` 都沒抓到 —— **尺壞了而讀數看起來像答案**（我差點據此下結論）。改成正經解析 PE → CLI header → metadata root → #Strings heap（354 個名字）才拿到可信清單。判準：**用土法拿到的否定結論（「沒有 X」）特別可疑，因為尺窄與真的沒有同形。**

**修法：SenateWindow.FeedKeyModifiers()，每幀自己補**
- 必須在 `m_Controller.Update()` **之前**：Update 內部跑 ImGui.NewFrame，而 NewFrame 才消化 AddKeyEvent 佇列。放後面 ⇒ Ctrl 慢一幀到，ImGui 看到 V 那一幀 Ctrl 還是 false ⇒ 快捷鍵永遠差一步且不報錯。
- `Mod*` 與實體左右鍵都餵（官方 backend 也是）：前者給快捷鍵判斷，後者給「哪一顆被按著」。
- 順便補 V/C/X/A/Insert：TranslateInputKeyToImGuiKey 有沒有涵蓋它們我沒有 IL 層讀數，而 AddKeyEvent 對「狀態沒變」是**幂等**的 ⇒ 重複餵不打架，漏掉才會壞。**在沒有讀數的地方選不會壞的那一邊。**

**新診斷基建：`ui --window --keydebug`**
「Ctrl+V 沒反應」有三個斷點且在畫面上長得一樣：①ImGui 收不到 Ctrl ②收到但沒呼叫 callback ③callback 被呼叫但剪貼簿空。
⇒ 畫面底部印 io.KeyCtrl / Silk:Ctrl / V / **clipboard callback Get/Set 計數** / WantTextInput。
⭐ 那個 **callback 計數器**是唯一能區分 ①② 的東西（加在 ImGuiClipboardBridge.GetCalls/SetCalls）。
⭐ 另加**不需按鍵的自我對拍**：第 4 幀注入 ModCtrl=true、第 5 幀讀回 io.KeyCtrl ⇒ 截圖實測 **讀回 True**，證明補 modifier 那條路本身有效，不必等人按鍵盤。

**順手修（截圖抓到的）**：「📋 貼上」的 📋(U+1F4CB) 在 seguisym.ttf 沒有 glyph ⇒ 畫成 `? 貼上`。換成 `⇣`。
📌 同字型判準：不是「字型有沒有載」，是「這一頁實際用到的每個字元有沒有 glyph」，而缺字不報錯。⚠ 我是從自己的截圖看到的 —— 只讀文字 renderer 的輸出這格永遠不現形。

**仍未驗**：真實鍵盤 → Silk IsKeyPressed → ImGui → InputText 這一整條（截圖模式沒有鍵盤事件）。已請 Tim 用 --keydebug 按一次，看 Get 是否 +1。非 Windows 的 pbpaste/xclip 也仍無讀數。
