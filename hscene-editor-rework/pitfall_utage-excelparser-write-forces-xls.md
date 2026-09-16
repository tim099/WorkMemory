---
id: pitfall_utage-excelparser-write-forces-xls
topic: hscene-editor-rework
title: ExcelParser.Write 強制改成 .xls ＋ OpenOrCreate 不截斷；重建 xlsx 要自己 XSSFWorkbook + FileMode.Create
type: pitfall
status: active
created_at: 2026-09-16
created_by: kaguya
links: [hscene-editor-rework/pitfall_npoi-editor-only-asmdef]
related_docs: []
---

**`Utage.ExcelParser.ExcelParser.Write(path, grid)` 不能用來寫 `.xlsx`，而它兩種失敗都不出聲。**

讀碼確認（2026-09-16，`Assets/Utage/Editor/ExcelParser/ExcelParser.cs`）：

① **L181 `path = FilePathUtil.ChangeExtension(path, ExtXls);`** —— 它**無條件把副檔名改成 `.xls`**，
   而 `MakeBook` 建的是 `HSSFWorkbook`（Excel 97 格式）。
   ⇒ 傳 `Sound.xlsx` 進去 ⇒ 旁邊生出一個 `Sound.xls`，**原檔一個位元組都沒動**。
   而 `Write` 回傳 `void` ⇒ **沒有任何一層會說「你要寫的那個檔沒被寫到」**。

② **`Write` 與 `WriteBook` 都用 `FileMode.OpenOrCreate` + `book.Write(fs)`** —— `OpenOrCreate` **不截斷**。
   ⇒ 新內容比舊檔短時，**尾巴殘留舊 bytes**，產出一個打不開的檔。

⇒ 修法：自己建 `XSSFWorkbook`（`.xlsx` 用它，不是 HSSF），並以 **`FileMode.Create`** 寫出（Create 會截斷）。

📌 另一半：`ExcelParser.Write` 走的是 `MakeBook` ⇒ **從 grid 重建整本**，
既有格式（欄寬、字色、背景）全部丟失。要保留格式得走 `ReadBook` → 改 `ISheet` 儲存格 → 自己寫回。
本次（Sound.xlsx）刻意選了「整份重建」⇒ 格式丟失是規格不是缺陷，但**做決定前要知道有這個岔路**。

⚠ 而「整份重建」有一格很容易漏：**先把人手填過的欄位讀出來、覆蓋後回填**。
🩸 Sound.xlsx 的 `Volume` 有 301 筆，其中 **16 筆不是 1.0**（人調過音量）——
不回填的話它們會被靜默打回 1.0，而重建出來的表**看起來完全正常**。
⇒ 回填的 key 要用**不會被這次改寫的欄位**（本例是 `(Type, FileName)`），
⛔ 不能用正要被重新推導的 `Label` —— 用它的話失敗樣子是「回填 0 筆」不是報錯。
