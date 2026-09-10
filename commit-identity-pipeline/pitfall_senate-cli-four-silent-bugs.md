---
id: pitfall_senate-cli-four-silent-bugs
topic: commit-identity-pipeline
title: 移植當天四隻不報錯的坑：漏搬守衛／兩側編譯不同判／submodule 盲區／porcelain 左trim
type: pitfall
status: active
created_at: 2026-09-10
created_by: gura
links: []
related_docs: []
---

2026-09-10 移植 `senate cmd commit` 時一天撞四隻，**四隻都不報錯**，全部靠探針或回讀抓到：

1. **漏搬兩道守衛**：打錯 persona 名時信箱一路掉到全域 fallback（**形狀完全合法的位址**）⇒ 組出 `?@打錯的名字(?) <fallback@…>` 然後若無其事提交。抓到它的是「拿它做第一次真提交之前先跑三個守衛探針」，而第三個回的是 **exit 4 —— 一個「正常」的出口**。⇒ **先量守衛，再量功能。**

2. **Unity 綠燈而 senate 紅燈**：`SCP_TaskEntry aHit = null;` 在 Unity（nullable 未開）完全合法，`dotnet build` 是 CS8600。⇒ **「編得過」不是全域事實，是對某個編譯器某組設定的答案。** 驗收條文要寫「兩側各自量」而不是「編譯通過」。

3. **`left_dirty_cs` 在真的有髒檔時回 0**：根層 `git status` **看不見 submodule 裡的檔**，而 Unity C# 幾乎全住在 `Assets/Plugins/SCP_Core`／`UCL_Core` 兩顆 submodule 裡。⇒ 那不是「範圍小」，是**錯的讀數**，而它跟乾淨一模一樣。修法：讀 `.gitmodules` 取 `Assets/` 底下的 submodule 逐顆掃並加路徑前綴。

4. **porcelain 路徑少一個字**：`Substring(3)` 對原始行是對的（`XY path` 固定 3 格），但 `SCP_Git.OutLines()` **會左 trim** ⇒ ` M path` 進來已是 `M path`，固定切 3 個字多吃掉第一個字元。同一段 code 抄自 `Cmd_AutoCommit`（那邊讀原始 stdout 所以沒事）。⇒ **同一段 code 換一個輸入來源就壞掉，而它不報錯 —— 它產出一條看起來很像真的的假路徑。** 修法：regex `^[ MADRCU?!]{1,2}\s+` 吃掉狀態欄，不假設它幾個字。
