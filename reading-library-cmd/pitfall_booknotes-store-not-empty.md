---
id: pitfall_booknotes-store-not-empty
topic: reading-library-cmd
title: 「舊 BookNotes book.json store 已空」是錯的註解 —— 磁碟 157 份、活的 6 份、5 天內寫過 2 次
type: pitfall
status: active
created_at: 2026-09-06
created_by: basecamp
links: [decision_python-recall-retire-gate]
related_docs: []
---

## 🩸 `UCL_BooksIO.cs:16 / :157` 註解宣稱「舊 BookNotes/<slug>/book.json 該 store 已空」—— **磁碟說不是**

**實測 2026-09-06（basecamp，TASK-0143 ② 開工第一格）**
host `Tim-PC` ／ repo `Bar` ／ root `D:/Unity/Bar/AgentCommands`：

```
find BookNotes -name book.json   ⇒ 157 份
  Archive/ 底下  151
  Library/ 底下    0
  活的（非 Archive/Library）  6
```

活的六份與 mtime：
- `anim-apocalypse-hotel/branches/calli/book.json`　2026-08-25
- `basecamp-foot-of-the-mountain/book.json`　2026-08-23
- **`book-gura-abyssal-verifications/book.json`　2026-09-01**
- `comic-delicious-in-dungeon/branches/gura/book.json`　2026-08-13
- **`sirius-night-lamp/book.json`　2026-09-04**
- `summit-masthead-bet/branches/gura/book.json`　2026-08-26

⇒ **不只不是空的，五天內還被寫過兩次**，涉及 calli／gura／Sirius／summit／basecamp 五個人。

## 為什麼要記成 pitfall 而不是順手改掉那兩行

那句註解**會被當成移植的前提**：任何人做 python 端退場時讀到「該 store 已空」，
就不會去問「那這些檔由誰接手」。⇒ 失效樣子是**靜默丟資料**，
而丟掉的形狀是「檔案還在、只是沒有人再讀它」——**不會有任何一層喊**。

## 動作型判準

⛔ **不准拿註解當資料現況的讀數。** 要宣告一個 store 空了，判準是
`find <store> -name <檔名> | wc -l`（並分開 Archive 與活的），不是「我記得它空了」。

## 順帶更正另一格（同一天量到）

工作記憶 `decision_python-recall-retire-gate`（`status: active`）記著
「`UCL_ReadingLibraryIO.cs:874` 與 `library.py:328` 寫同一路徑、漂移方向是內容變少」。
**那個漂已經關閉**：`library.py` 的 `recall` 零命中，兩個行號現在指著完全不同的 code，
且兩邊寫入落點互斥（python 寫 `book.json`；C# 對它只有 `LoadJson`）。
⇒ **記憶是對的，只是它已經被解決而狀態沒有人翻** ——
而我開 TASK-0143 條文時**拿它當現況寫進了驗收標準**，沒有先驗。
📌 判準：**`status: active` 是寫的時候的狀態，不是現在的狀態。**
