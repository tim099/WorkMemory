---
id: pitfall_writer-side-swap-cannot-close-window
topic: agentcmd-queue-lost-update
title: 寫入端換法關不掉窗口；Move(overwrite) 兩側都不可用（而我判錯的成因是讀錯專案的 TargetFramework）
type: pitfall
status: active
created_at: 2026-09-23
created_by: kaguya
links: []
related_docs: []
---

**寫入端換法關不掉那個窗口，而「唯一關得掉的那個 API」兩側都不存在 —— 別再試第三次。**

TASK-0265 原本的受詞是寫入端（「有害的那幾處改 `File.Replace`」）。2026-09-23 逐一量完，三條路全斷：

| 換檔寫法 | 讀取端（**stat** 受詞）讀到「不存在」 | 可用性 |
|---|---:|---|
| `Delete` 然後 `Move` | 40.9 / 40.9 / 40.8 %（三輪） | 現況 |
| `File.Replace`（無備份／帶備份） | 6.5 % ／ 6.7 % | 可用 |
| `File.Move(…, overwrite: true)` | **0.0 %**（三輪） | 🔴 **兩側都不可用** |

⛔ **`Move(overwrite)` 不要再試**：
- Unity 側：`error CS1501: No overload for method 'Move' takes 3 arguments`
- Senate 側：`SCP_Core.csproj` 釘在 `netstandard2.1` / C# 9 —— 而那是**刻意的**，
  它的檔頭逐字寫著「這個 csproj 最重要的功能不是編出 dll，是**把方言限制變成編譯錯誤**」。

🩸 而我為什麼會以為它可用（同一天犯兩次）：
**我讀了「一個」專案的 TargetFramework，拿它去判「那個」專案** ——
Unity 側讀 `ProjectSettings` 的 `apiCompatibilityLevel`；Senate 側讀 `Senate.Cli.csproj` 的 `net10.0`
＋ `Directory.Build.props`，而 `SCP_Core.csproj` 自己另有一格。

⇒ **判準：要用一個 API 之前，先確認「我要改的那個檔屬於哪個 csproj / asmdef」，讀那一份的 TargetFramework。**
⛔ 不要讀 solution 層或 props 層 —— 那兩層會給一個更寬、而且看起來很權威的答案。
⭐ 最便宜的驗法是「先寫一行編一次」（成本 9 秒），我第二次就是這樣抓到的。

⚠ **而上表那一欄的受詞要跟著 `pitfall_instrument-object-mismatch` 一起讀**：
我量的是 `File.Exists`（stat），而正主讀取端做的是開檔（`ReadAllText`）。
⇒ 📌 **那幾個百分比證明了方向（換寫入端只能縮小窗口），⛔ 它們不是開檔那一側的量級。**
開檔那一側的失效樣子是 `Sharing violation`，而那條路更兇（會被路由進 `Unreadable`）。
