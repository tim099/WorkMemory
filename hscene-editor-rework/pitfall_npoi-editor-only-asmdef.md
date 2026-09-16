---
id: pitfall_npoi-editor-only-asmdef
topic: hscene-editor-rework
title: NPOI/Utage.ExcelParser 在 Editor-only asmdef ⇒ Assembly-CSharp 引用不到（#if UNITY_EDITOR 救不了）
type: pitfall
status: active
created_at: 2026-09-16
created_by: kaguya
links: []
related_docs: []
---

**`Assembly-CSharp` 引用不到 NPOI，即使整段包在 `#if UNITY_EDITOR` 裡。**

NPOI 掛在 `Assets/Utage/Editor/ExcelParser/UtageExcelParser.asmdef`，那份宣告
`"includePlatforms": ["Editor"]` ＋ `"autoReferenced": true`
⇒ **只有 `Assembly-CSharp-Editor` 會自動引用它**；`Assembly-CSharp` 不會。
而 `#if UNITY_EDITOR` 只決定「這段在 Editor 下編不編」，⛔ **不會把它換到另一個組件去**。

🩸 實測（2026-09-16，TASK-0228）：
- `new NPOI.XSSF.UserModel.XSSFWorkbook()` 放進 `Assets/Scripts/UCL_Assets/`（Assembly-CSharp）
  ⇒ `error CS0246: The type or namespace name 'NPOI' could not be found`
- **同一行**搬到 `Assets/Scripts/Editor/`（Assembly-CSharp-Editor）⇒ 過。
  順帶量到：同一份探針裡 `Utage.ExcelParser.StringGridDictionary` 仍報 CS0234
  ⇒ 那個型別不在該 namespace，**跟組件引用不到是兩回事**，別混為一談。

⇒ 而 `HSceneAsset` 是 `partial class` 且必須留在 Assembly-CSharp（它是 runtime 資產類），
而 **partial 不能跨組件** ⇒ 想在 HSceneAsset 的 Editor 區塊裡寫 xlsx，只剩一條路：
把實作放 Editor 資料夾，HSceneAsset 那側留一個**反射薄殼**
（`Type.GetType("<全名>, Assembly-CSharp-Editor")` + `GetMethod` + `Invoke`）。
⚠ 薄殼找不到型別時**一定要出聲**：反射斷掉的失效樣子是「按了按鈕什麼都沒發生」，
而它跟「掃出零筆」在畫面上完全同形。

📌 驗這件事最便宜的方法是**丟一個三行探針編一次**，⛔ 不要靠讀 asmdef 推。
⚠ 而第一趟 `unity-recompile` 回的是 `compile_errors=0 / verdict=clean` —— **假的**，
因為探針還沒被 import（同一份回傳檔的 `stale_sources=1` 就是證據）。
喊出來的是 `unity-compile-status` 的 ErrorLog 對帳。⇒ **stale_sources>0 時那個 errors 數字不是你的。**
