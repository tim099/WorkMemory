---
id: pitfall_compile-report-scope-per-assembly
topic: hscene-editor-rework
title: check_compile 報告只涵蓋本次重編的 assembly — warning 數跨 pass 不可比、in-progress 的 Errors:0 不是證據
type: pitfall
status: active
created_at: 2026-08-21
created_by: Sirius
links: []
related_docs: [Assets/Plugins/UCL_Core/Tools~/AgentCommands/check_compile.py]
---

`check_compile.py` 的報告**只涵蓋「本次重編的 assembly」**，所以 warning 數在不同 pass 之間不可比。

2026-08-21 實測同一天三次讀數：
- 11:26（只動 Assembly-CSharp 的檔）→ Warnings **13**，全是 Assembly-CSharp 的 CS1998/CS0105/CS0414
- 11:52（改完大批 Assembly-CSharp）→ Warnings **0** / Total messages **0**
- 11:55（也動了 UCL_Core 的檔）→ Warnings **36**，全是 UCL_Core 的 CS0618/CS0162/CS0108

## 兩個會騙人的地方
1. **`Warnings: 0` 不代表乾淨、也不代表沒編**；它可能只代表這次重編的 assembly 沒有 warning。
   反過來 warning 從 13 跳 36 也不代表「我弄壞了 23 個地方」—— 要看 assembly 欄與檔名。
2. 報告開頭可能印 **`⏳ Compile in progress — 結果尚未定案`**，而它底下照樣印 `Errors: 0`。
   那個 0 不是證據。要輪詢到那行消失，**並且**比對 report timestamp 晚於自己最後一次寫檔（逐秒，不是逐分）。

## 手勢
改完 .cs → `run Recompile` → 輪詢到非 in-progress → 比 timestamp vs `find -newermt` 的最後改動時間
→ 若 timestamp 沒前進，送 `AssetDatabase.Refresh(ImportAssetOptions.ForceUpdate)` 再輪詢。
最後用**獨立訊號**收尾：`Cmd_Invoke` 打新型別／新成員，回傳型別對得上才算真的編過。
