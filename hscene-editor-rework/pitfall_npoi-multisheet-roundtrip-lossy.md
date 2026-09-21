---
id: pitfall_npoi-multisheet-roundtrip-lossy
topic: hscene-editor-rework
title: NPOI 開既有多表活頁簿只改一張表再寫回：其他表列數不變而內容被改掉（而只比對列數的守衛會印假綠燈）
type: pitfall
status: active
created_at: 2026-09-21
created_by: calli
links: []
related_docs: []
---

## 讀數（2026-09-21 實測，變因單一）

用 NPOI 開既有的 `LittleYellow.xlsx`（15 張表）→ 只改 `Texture` 那一張 → 整本 `Write` 回去。
外部逐列 hash 對拍（python，不是 NPOI 自己）：

| 表 | 列數 | 內容 hash |
|---|---|---|
| Texture | 13→9 | 變（預期內） |
| **Character** | 33→33 | 🔴 **變了** |
| **Credits** | 5→5 | 🔴 **變了** |

⇒ **NPOI 的 round-trip 不是無損的。** 「開既有活頁簿、只改一張表、其餘原樣寫回」這條路不成立。

## ⚠ 而更貴的是：我的守衛漏報了

當時的檢查是「表名 ＋ 非空列數」逐張比對 ⇒ 列數沒變 ⇒ 它印 `✓ 其餘工作表逐張列數不變`。
**一個假綠燈。** 抓到災情的是一把不同源的尺（外部逐列 hash），不是那份普查。

📌 判準：**回讀要讀到值，不能只讀計數。** 計數相同是內容相同的必要條件，不是充分條件，
而兩者在報告上長得一模一樣。

## 處置

多表活頁簿**不要程式化重寫**。要自動產出就輸出**獨立單表檔**（比照 `Sound.xlsx`）：
`new XSSFWorkbook()` ＋ `FileMode.Create`，那條路沒有其他表可以被波及。
（Tim 2026-09-21 拍板 Texture／Character 各自獨立成 `Texture.xlsx` / `Character.xlsx`。）
