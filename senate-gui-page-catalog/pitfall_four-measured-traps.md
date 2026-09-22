---
id: pitfall_four-measured-traps
topic: senate-gui-page-catalog
title: 四個實測踩坑：彈窗貼錯標記／PageKey 不繼承／chcp 無效／永遠綠的燈
type: pitfall
status: active
created_at: 2026-09-22
created_by: summit
links: []
related_docs: []
---

## ① 貼錯標記的代價：彈窗頁會**活不過 CLI 的下一次呼叫**

`SCP_GuiPageController.PathKeys` 存的是**每一層的 Key**（含彈窗那層），而 CLI 每次呼叫都是新 process
⇒ 下一趟走 `RestorePath`，它對每個 key 叫一次 `catalog.Create`；回 null 就照 **D13** 的規矩
**停在那一層並回報**（⛔ 不跳過繼續往上疊）。
⇒ 把彈窗頁貼成 `[SCP_PageIgnore]`（不收錄）⇒ `Create` 回 null ⇒ **在彈窗裡按第二下，人已經掉回母頁了**。
⚠ 而「彈窗選項頁按下去」就是第二步 ⇒ **主路不是邊角**。
📌 射程：**只有 CLI／文字模式**；視窗模式導覽狀態活在記憶體裡，不走 `RestorePath`。
📌 反過來也一樣：探針改用 `MenuGroup => null` 也不對 —— 它會被收錄、ctor 會被呼叫、`--page` 叫得到。

## ② 子類**不會**繼承基底的 `PageKey`（實測，別重查）

`Type.GetField("PageKey", Public|Static)` **不帶 `FlattenHierarchy`** ⇒ 不撈繼承來的靜態成員。
⇒ 基底宣告 `public const string PageKey` 時，**子類不會全部撞名**；它們拿到的是「沒有 PageKey」的缺陷
—— 而那個要求是對的：每個具體頁本來就該有自己的 key，忘了宣告會在 **build 階段**被 `pages-check` 點名。
⚠ 抽象基底則**完全不產生缺陷**（掃描本來就跳過 `IsAbstract`）。

## ③ MSBuild 的 `Exec` 會把中文吃成亂碼，而 `chcp` **沒用**

缺陷訊息在 build log 裡長這樣：`繚 page key ... 鋡怠???亙恐??`（系統字碼頁解那條管線）。
⛔ 試過 `chcp 65001 && dotnet …`：**無效**（chcp 動的是 console，不是 MSBuild 讀的管線）
⇒ **沒有留那一行** —— 一個沒作用的修法比沒有修法糟，它會讓下一個人以為這格處理過了。
⭐ 解法在程式側：失敗時**先印一段純 ASCII 摘要**（key 與 TypeFullName 本來就是 ASCII）：
`[pages-check]   page key \`bank\` \`Senate.Cli.Pages.BankAdminPage\` \`Senate.Cli.Pages.PathsPage\` …`

## ④ 舊的 `PageDiscovery` 對拍不能留

它驗的是「程式碼 vs 目錄」的**差集**，而自動收頁之後目錄就是程式碼 ⇒ 它會永遠回 0 筆，
而 0 筆現在的意思是「對得上」。⛔ **一個永遠綠的燈比沒有燈危險 —— 它看起來像有人在看。**
⇒ 整支改名 `PageAutoRegister`，重新指向撞名／ctor 形狀／標記／標籤四件事。
⚠ 其中三格的受測體**必須是合成的**（產品型別上造不出撞名），所以 `AutoRegisterTypes` 收一個
`iIncludeIgnored`（只有對拍會給 true）—— 理由寫在它的 XML 註解裡。
