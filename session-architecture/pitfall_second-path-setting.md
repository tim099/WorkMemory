---
id: pitfall_second-path-setting
topic: session-architecture
title: 加一格路徑設定之前先問「這個值系統裡是不是已經存著了」—— 我造了第二份，而第一份就在旁邊
type: pitfall
status: active
created_at: 2026-09-04
created_by: basecamp
links: []
related_docs: []
---

2026-09-04，Tim 抓到的（basecamp wake#89）。

## 現場

TASK-0127 ⑥ 我讓 `SCP_GuiSessionAdminPage` **自己存一格手填的資料根**（`sessions/dataRoot` pref，
落在 `senate.pages.local.json`）。而 `SCP_PathId.AgentCommandsRoot` **早就是**那個統一設定：
Global、只有一組、存在 `senate.local.json`、由「路徑管理」頁編輯、`auto` 從專案根推導。

⇒ 症狀不是重複而已：**第二份可以跟第一份說不一樣的話**，
而那時本頁會讀到另一棵樹的 session，**然後每一列都顯示正常**。

## 🩸 最難看的那一格：我把自己的 bug 讀成了設定的缺口

那一頁印「還沒設定資料根」的時候，**整個 CLI 早就解得出那個根**（每支 cmd 都印 `data_root=…`）。
我讀到那句話的反應是「設定沒填」⇒ **去寫使用者的 prefs 才把那一頁『驗完』**。
⇒ 一個自己造的洞，被我用「填補使用者的設定」蓋過去，而沿途沒有一格會紅。

## 為什麼我沒發現（射程錯在哪）

我在那個 pref 的註解裡替它辯護過：「資料根**顯式設定**，不從信件夾根推導」——
**那句話是對的**（從 lettersRoot 反推 dataRoot 是錯的推導）。
錯的是我排除了**錯的推導**之後，**沒有去問對的那一格是不是已經存著了**。
📌 一般形：**「我證明了 A 不對」不蘊含「所以要自己造一個」** —— 中間漏掉的是「既有的是什麼」。

## 動作型修法

1. 加任何一格路徑設定之前，先 grep `SCP_PathId` 的成員清單（那是唯一那份描述表）。
2. 頁面要讀那個值：走宿主介面（`ISCP_GuiAppContext.AgentCommandsRoot`，回 `SCP_PathResolution`
   ⇒ 值／來源／取不到的原因**三態不同形**）。⛔ 不要在頁面存路徑。
3. 已經有先例可抄：`SCP_GuiLoginStatusPage` 的 `awakening.lettersRoot` 走 `SenateAwakeningPrefs`
   **轉接到同一個檔**（那個檔的檔頭寫著「一個檔只能有一個寫入端」）—— 我該抄的是它。
4. 判準（`PathsPage.cs` 檔頭那句）：**能被推導或已經被存過的路徑，不准再存第二份。**
   存了就是給漂移一個住的地方。

## ⚠ 同族還沒查的一格（留給下一個人，不是待辦）

`SCP_GuiLoginStatusPage` 讀的是 pref 的**原始值**，而 `lettersRoot` 在描述表裡**支援 `auto`**
⇒ 若有人把它設成 `auto`，那一頁會顯示字面的 `auto` 而不是解出來的路徑。
我沒有量它（沒有人把它設成 auto），所以這是**線索不是讀數**。
