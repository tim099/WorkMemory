---
id: state_state-day2
topic: senate-backend
title: Day 2 現況：顯示參數／頁面堆疊／反射三層都上了，Unity 端仍零讀數
type: state
status: active
created_at: 2026-08-23
created_by: basecamp
links: [senate-backend/state_state-day1]
related_docs: [D:/Unity/Senate/Docs/DOC_INDEX.md, D:/Unity/Senate/README.md]
---

2026-08-23（basecamp wake #69）Senate 後台這條線推進到：

**做完並實測過的**
- 顯示參數統合層 `SCP_GuiStyle`（scale/間距/字級/顏色/文字寬一份資料）＋通用 `SCP_Color`。
  預設 scale 定案 **1.0**（Tim 在真視窗按過四段後選的；我第一版推成 2.0 —— 方向對幅度錯）。
- 頁面堆疊 `SCP_GuiPage` / `SCP_GuiPageController`：**一個 Window 一套 controller，沒有全域單例**；
  同一個 page 實例 push 兩次丟例外；導覽路徑存進 session 的 `nav`（CLI 每次都是新 process）。
- 反射三層：`SCP_Reflect`（快取）→ `SCP_TypeSchema/SCP_MemberSchema`（九種 ValueKind ＋ [SCP_Ignore]）
  → 兩個消費端吃同一份（`SCP_JsonMapper` / `SCP_GuiInspector`）。
- 摺疊 `SCP_Ui.Fold`：狀態住 `SCP_GuiInput.Folds` / session，**收合時子節點不建**；CLI `--fold <id>`。
- 版位：欄位名稱畫在左邊（`LabelLeft`，對齊 `LabelWidth`；標籤過長不裁字直接推開）。
- 設定頁改成自動繪製**整份 senate.local.json**（config → projects[] → 欄位，真三層）—— 零欄位碼。
- CLI 新旗標：`--scale` / `--size` / `--fold` / `--page`（`--page` 是驗收出口：截圖模式沒有點擊入口）。
- selftest 十項全綠（含頁面堆疊語意、摺疊語意、schema 分類、序列化 round-trip、自動繪製真的寫進物件）。

**踩過並修好的（都寫進 Decisions D11–D15）**
- 設定檔寫回去吃掉使用者手寫的 `"//"` 註解（未知欄位沒接住 ＋ 中文被轉義）⇒ `[JsonExtensionData]` ＋ `UnsafeRelaxedJsonEscaping`，並立了 selftest。
- `--scale` 的覆寫套在 Draw 之後 ⇒ 警告說夾成 4、畫面印 2（尺寸的讀數也會說謊）。

**還沒有讀數的一格（明天第一件）**
- **Unity 端沒編過這批新檔**（頁面堆疊＋反射三層）。驗法：Bar 的 SCP_Core 拉到 master 再 `check_compile`，
  重點看 `init` 存取子與自補的 `IsExternalInit` polyfill 在 Unity C#9 吃不吃。
- Senate 的 `origin/master` 上有 Tim 的 `d65f2d7 fix(sln)`，我本地 5 筆長在它之前 ⇒ 要接上去得 rebase。
- 兩個舊缺口沒動：文字 renderer 的表格不吃 `--width`；中文 IME 沒實測（畫面上還沒有輸入框可打字）。
