---
id: pitfall_gui-font-binding-traps
topic: senate-backend
title: Senate 視窗字型三坑：ImFontGlyph 版面錯、Silk BGRA、explorer 正斜線
type: pitfall
status: active
created_at: 2026-10-02
created_by: apex-one
links: []
related_docs: [src/Senate.Desktop/SenateEmoji.cs, src/Senate.Desktop/SenateFonts.cs, src/Senate.Core/SenateShell.cs]
---

Senate 視窗（ImGui.NET 1.90.8.1 ＋ Silk.NET 2.23）三個「不報錯、看起來還行」的坑（2026-10-01，TASK-0356／開啟資料夾）：

1. **`ImFontGlyph` 綁定版面是錯的**：bitfield `Colored:1 Visible:1 Codepoint:30` 被生成成三個 uint（48 bytes，原生 40）。
   ⇒ 要改字形旗標一律從 `NativePtr` 直接寫原生第 0 位元，⛔ 不經 `ImFontGlyphPtr.Colored`（會寫到別的欄位）。量法：reflection `Marshal.OffsetOf`。
2. **Silk 的字型貼圖是 BGRA 上傳**（`Texture..ctor`：RGBA8 內部格式／BGRA 像素格式，讀 IL 常數 0x80E1 量到）。
   白色字形對調看不出來，彩色像素 R／B 互換。⇒ 寫 atlas 的彩色像素要按 BGRA 排。
3. **explorer.exe 不認正斜線**：SCP 路徑是 `D:/…`，原樣丟進去會開到使用者的「文件」，視窗照樣跳出來。
   ⇒ 已修在 `SenateShell.ExplorerArgs`（`Path.GetFullPath`）＋ selftest `ExplorerArgsBackslash`。新的「開檔案總管」路徑一律走它。

另：16 位元 ImWchar 下的 emoji 走 `SenateEmoji`（私用區借位＋COLR v0），各頁零修改；改字型載入順序時注意 `AddEmoji` 必須在 Silk 上傳前（onConfigureIO 內）。
