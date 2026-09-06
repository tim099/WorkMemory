---
id: state_inventory-20260906-library-py-35-subcmds
topic: reading-library-cmd
title: library.py 35 支子命令退場盤點表（TASK-0143 ①）—— A 已有 C#/B 要移植/C 待議/D 零引用候選
type: state
status: active
created_at: 2026-09-06
created_by: basecamp
links: []
related_docs: []
---

## 📋 library.py 35 支子命令 —— 退場盤點表（TASK-0143 ①，basecamp 2026-09-06）

📍 host `Tim-PC` ／ repo `Bar` ／ ref `master` ／ `library.py` 3289 行。

### 讀數怎麼拿到的（三欄各自的出處，⛔ 不同源）

| 欄 | 量法 |
|---|---|
| **C# 對應** | `grep -oE 'case "[a-z_]+"'` 掃 **實際 dispatch**（⛔ 不是 `ArgsSchema` 的字面 —— 那是說明不是行為）。`Cmd_Library` 9 個 op、`Cmd_Books` 8 個 op，共 **17**。 |
| **活引用** | 錨在 `library.py` 之後 120 字元內（含跨行續行），掃 **79 個活檔**（排除信件庫／BugReports／Tasks／inbox／Archive／completed Plan）。 |
| **判定** | 由前兩欄推得，**每一格都標了為什麼**。 |

⛔ **這張表是名字層，不是行為層。** 「C# 有同名 op」**不代表行為等價** ——
前例（`decision_python-recall-retire-gate`）：reading-recall 兩版 6308 / 4210 / 1973 bytes，
**不等價、不是格式差是資料差，且互有對方沒有的節**。
⇒ 每一支標「python 退場」的，退場前都要過條文 ⑥ 的逐位元組對拍。

---

### A. C# 已有同名 op（9 支）⇒ 判定「**python 退場**」，⚠ 前置＝行為對拍

| py 子命令 | C# 對應 | 活引用 | 註 |
|---|---|---|---|
| `add-character` | `Cmd_Library.add_character` | 2 | |
| `revise-view` | `Cmd_Library.revise_view` | 1（只有 `library.py` 自己）| **零外部呼叫端** |
| `bookmark` | `Cmd_Library.bookmark` | 2 | |
| `log-chapter` | `Cmd_Library.note_chapter` | 4 | ⚠ **名字不同**（log-chapter ↔ note_chapter）⇒ 對拍優先 |
| `donate` | `Cmd_Books.donate` | 2 | 🔶 含金流 ⇒ 委派 |
| `publish` | `Cmd_Books.publish` | 1 | 🔶 含金流 |
| `tip` | `Cmd_Books.tip` | 2 | 🔶 含金流 |
| `tips` | `Cmd_Books.tips` | 1 | |
| `shelf` | `Cmd_Books.shelf` | 2 | ⚠ 與 `shelf-update` 是兩支，C# 只有一個 `shelf` ⇒ 要確認 C# 那支涵蓋哪一半 |

### B. **要移植**（無 C# 對應 ＋ 有活引用）—— 3 支，但份量集中在第一支

| py 子命令 | 活引用 | 為什麼要移植 |
|---|---|---|
| **`export-watch`** | **15**（含 `Cmd_StreamWatch.cs:3460` 那條 code 橋）| ⭐ **活路徑上唯一的 python 依賴**。移植完 `ProcessStartInfo` 才退得掉。 |
| `list-untitled` | 1 | 與 `export-watch` 同一族（哨兵章查法）⇒ 同批移植 |
| `add-book` | 8 | ⚠ 疑似與 `Cmd_Library.media_init` 重疊（後者＝建 work/media ＋ 登記 reader）⇒ **待對拍，可能其實屬於 A 類** |

### C. **無 C# 對應、活引用 1～5** ⇒ 待議（逐支要人確認，⛔ 不自己判死）

| py 子命令 | 活引用 | 一句話 |
|---|---|---|
| `branches` | 5 | 列某書的閱讀分支 |
| `stt-prompt` | 4 | 組 whisper initial_prompt（陪看 STT 用）—— ⚠ 與 `set-name-original` 成對 |
| `show-book` | 1（自引）| 顯示書本概覽 |
| `resume` | 1 | 續讀 catch-up |
| `list` | 2 | 列出所有書 |
| `migrate-tips` | 1（`Cmd_Books.cs` 提及）| **一次性遷移**（T-BOOKS-STORAGE Phase A）⇒ 強退場候選 |
| `prepare` | 1（`Cmd_StreamWatch.cs`）| 🩸 **名字撞車**：py 的 `prepare` ＝找書候選報告閱讀覆蓋；C# 的 `step=prepare` ＝觀影準備。**兩件事**。⇒ 這一格本身就是「同形」風險，退場時要確認沒有人把兩者讀成同一個。 |

### D. **零錨定引用**（15 支）⇒ 退場候選 —— ⛔ **不是退場清單**

`volumes` `terms` `tag` `show-character` `shelf-update` `set-name-original`
`search` `reviews` `recommendations` `recommend` `donations` `arcs` `arc`
`add-volume` `add-term`

⚠ **「0」的射程**：這 79 個活檔裡沒有錨定得到的引用。
**不是**「沒有人用」—— 有人手打一行指令是量不到的。
⇒ 退場前要過條文 ① 的第二關：**至少一位其他 persona 確認沒在用**。

📌 三格值得單獨點名：
- `donations` 零引用，**而 `Cmd_Books` 有同名 op** ⇒ 它其實屬於 A 類（C# 已有）。
- `set-name-original` 零引用，但它是 `stt-prompt`（4 引用）的**資料前置** ⇒ 成對處置，別只退一支。
- `arc` / `arcs` 是「每 ~6 章一個見林總結」—— 那是**閱讀側的見林**，跟 persona 的見林同名不同物。

---

### 📊 收斂後的數字

| 類 | 支數 | 判定 |
|---|---|---|
| A. C# 已有同名 op | 9（＋`donations` ⇒ **10**）| python 退場（前置：行為對拍） |
| B. 要移植 | 3 | `export-watch` 是重心 |
| C. 待議 | 7 | 逐支要人確認 |
| D. 零引用退場候選 | 15（扣掉 `donations` ⇒ **14**）| 需第二個人確認 |
| **合計** | **35** ✅ | 對得起來 |

⇒ **真正要寫的新 code 集中在 B 的 3 支**，而其中 `export-watch` 一支就佔了 15 個活引用中的全部 code 橋。
⛔ 而它現在是 @summit 的工地（TASK-0061，118 行未提交）⇒ 條文 ⑤ 已寫死「不可以從這裡起手」。

### ▶ 這張表之後的下一步（照條文順序，不是照方便）

**②「先關掉正在漂的那格」** —— `UCL_ReadingLibraryIO.cs:874` 與 `library.py:328` 寫同一路徑，
2026-08-07 記到現在沒關，漂移方向是「內容變少」。
⇒ 那一格**不需要等 summit**，而且它排在任何新實作之前。**開工從那裡開始。**
