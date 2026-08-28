---
id: knowhow_imgui-clipboard-bridge
topic: senate-backend
title: ImGui 剪貼簿 callback 接法：八條判準＋三層驗收（第三層刻意留白）
type: knowhow
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/pitfall_typed-field-per-char-rescan-and-clipboard]
related_docs: [D:/Unity/Senate/Docs/Architecture/Ui_Framework.md, D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

接續 pitfall_typed-field-per-char-rescan-and-clipboard 的第③點：那顆「📋 貼上」鈕只是繞道，Tim 要求真的接上。已完成。詳見 Senate Docs/Logs/Decisions.md D19 ⑩ 與 Docs/Architecture/Ui_Framework.md「剪貼簿」章。

**形狀（一份實作、三個消費端）**
ImGui(Ctrl+C/Ctrl+V) → io.SetClipboardTextFn/GetClipboardTextFn → ImGuiClipboardBridge(Senate.Desktop, marshalling) → SenateClipboard(Senate.Core, Windows Win32 CF_UNICODETEXT / 其他平台委給 SenateShell 的 process 路徑) ← SCP_GuiHost.ReadClipboard/CopyToClipboard(貼上鈕、複製類別名鈕)
⇒ 不會有「鈕能貼、Ctrl+V 不能」的分岔。順手把 CopyToClipboard 從 clip.exe 換成同一條路。

**版本關鍵**：ImGui.NET 是 **1.90.8.1** ⇒ callback 在 `io.SetClipboardTextFn` / `io.GetClipboardTextFn`。**1.91 之後搬到 `ImGui.GetPlatformIO().Platform_*`** —— 升版要跟著改，而接錯地方的症狀是**靜默無效**（所以 Install 會把指標讀回來並回報一行診斷）。

**八條判準（每一條寫錯的症狀都不是編譯錯誤）**
1. Windows 走 Win32 不走 clip.exe/PowerShell —— callback 是「按下組合鍵那一幀」，process 啟動 300-500ms 會被讀成視窗當掉
2. delegate 存 **static 欄位** —— GetFunctionPointerForDelegate 不會讓 delegate 活著；放區域變數 ⇒ GC 回收後某次貼上跳進非函式記憶體 ⇒ 隨機 crash，且離安裝那行很遠
3. 回給 ImGui 的 buffer 在**下一次**讀取時才釋放 —— 讀完立刻釋放＝在它還在讀時把地板抽掉
4. UTF-8 尾巴補 NUL —— C 端靠它判長度，少一位元組就讀過頭
5. callback 裡絕不讓例外飛出去 —— native→managed 邊界是 UB，ImGui 沒地方接；最壞回 IntPtr.Zero
6. SetClipboardData 成功後**不**釋放那塊記憶體 —— 所有權已轉移給系統，釋放的症狀是別的程式貼出垃圾
7. OpenClipboard 重試 6 次 —— 剪貼簿是全機唯一資源，輸入法/剪貼簿管理員開著時第一次會失敗（常態不是錯誤）；不重試的症狀是「Ctrl+V 有時候沒反應」，間歇性失敗最難被回報
8. 「空的」/「讀不到」/「裡面是圖片」三種分開回報 —— 壓成空字串 ⇒ 壞掉的能力看起來像「使用者沒複製東西」，人會一直重按

**驗收分三層，第三層刻意留白**
1. `selftest --clipboard` 的 `剪貼簿 round-trip`：SenateClipboard 寫入→讀回逐字相同（26 字元，含中文與符號 —— Win32 是 UTF-16、ImGui 端是 UTF-8，中間兩次轉碼；只用 ASCII 測會等到有人貼中文路徑才炸）
2. 同上的 `ImGui 剪貼簿 callback`：走**兩個 callback 本身**（Set 寫→Get 讀），36 字元 / 48 位元組逐字相同 ＋ UTF-8 結尾真的有 NUL
3. **「ImGui 真的會在 Ctrl+V 時呼叫它」要人親手按一次** —— 程式驗不到（截圖模式沒有鍵盤事件）
⇒ 第 3 層在 selftest 的讀數字串裡**寫明它沒被驗**，不讓「兩項全過」被讀成「Ctrl+V 一定能用」。這是 summit 憲法判準⑤「窄報/寬報」那一格：沒有代價的樂觀宣稱不會自己停下來。
另外開窗時印一行 `剪貼簿：已接上 ImGui（Ctrl+C / Ctrl+V 可用）` ＝ 兩個函式指標讀回來都非零。

**`--clipboard` 是 opt-in**：它會覆蓋使用者的剪貼簿，而那不可逆（舊內容可能是圖片，寫回也還原不了）。一個「跑一下自我檢查」的指令不該有這種副作用。

**我量錯一次，當場翻**：驗「預設 selftest 不跑剪貼簿」時用 `grep -c 剪貼簿` 回 1，而那 1 是別的項目讀數裡「沒有碰真的剪貼簿」那句 —— 尺太寬。改 grep 完整項目名後回 0。

**還沒有讀數的**：非 Windows 的 pbpaste/xclip 路徑（手上沒有那些平台）。
