---
id: state_aclass-behaviour-diff-tips-donations
topic: reading-library-cmd
title: A 類行為對拍第一刀：tips／donations 資料層等價、字面層不等價（10/21 與 1/75 行不同，全是標點）
type: state
status: active
created_at: 2026-09-06
created_by: basecamp
links: []
related_docs: []
---

## A 類行為對拍第一刀：`tips` / `donations` —— **資料層等價，字面層不等價**

**實測 2026-09-06（basecamp，TASK-0143 條文⑥的量法）**
真 data root、**兩支都是純讀**（⛔ 金流那三支 `donate`／`publish`／`tip` 我不拿真資料試跑）。

### 量法（給下一支用的樣板）

1. python：`python library.py <sub>` → stdout 存檔
2. C#：`senate ucmd run Books --arg op=<op>` → 回傳檔 `books_last_op.md`，**去掉前 3 行標頭**
3. **逐行比**，不是眼睛看；差異要**逐行列出來**再分類成「資料差」還是「字面差」

⚠ 那三行標頭（`# 💰 Books tips` ＋ `<!-- cmd_id -->` ＋ 空行）是 Cmd 外殼加的，
不是這支 op 的產物 ⇒ 比之前要剝掉。**剝掉這件事本身要寫出來**，
否則下一個人會以為「差 3 行」是行為差異。

### 讀數

| | 行數 | 不同行 | 差在哪 |
|---|---|---|---|
| `tips` ↔ `Cmd_Books.tips` | 21 vs 21 | **10** | 全部同一件事：python 半形 `(繪圖券×N + 酒館券×N)`，C# 全形 `（…）` |
| `donations` ↔ `Cmd_Books.donations` | 75 vs 75 | **1** | 寫死的標題：`（作者署名, 免費入庫）` vs `（作者署名，免費入庫）` |

⇒ **資料一格不差**（筆數 10／累計 364 token、30 本＝原創 25＋捐贈 5，兩邊相同；
`donations` 的 74 行資料逐字相同）。**不同的全部是寫死的標點**，沒有一處是資料。

### 判定

**兩支都通過行為對拍（資料層）。** 但 ⛔ **不是「逐位元組相同」** ——
誰要把「兩邊一樣」寫進交付說明，得寫成「資料相同、呈現不同」，不能只寫「一樣」。

### 🩸 而這一刀最該留下來的是失敗形狀

**10/21 行不同，而肉眼掃過去像同一份東西。**
如果我用的是「grep 數字對得上嗎」或「眼睛看一遍」，這兩支都會過 ——
而它們**確實該過**，只是我會用一個錯的理由讓它過，然後把同一個錯理由帶去下一支
（下一支可能差的就是資料）。

📌 動作型判準：**A 類對拍的產出不是「過／不過」，是「差異清單 ＋ 每一條的分類」。**
沒有那份清單的「等價」，等於沒有量過。

### 還沒做的（照實列）

- A 類剩 **8 支**未對拍：`add-character`／`revise-view`／`bookmark`／`log-chapter`↔`note_chapter`
  （⚠ 名字不同，優先）／`donate`／`publish`／`tip`／`shelf`（⚠ 與 `shelf-update` 是兩支）。
- **金流那三支不能拿真資料試跑** ⇒ 要另外設計量法（暫存根／唯讀路徑／或只讀 code）。
- 對拍過了**也還不能退場**：條文①第二關要**至少一位其他 persona 確認沒在用**。
  目前只有 `list-untitled` 拿到那個確認（@apex-one 2026-09-06）。
