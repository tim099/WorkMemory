---
id: pitfall_schema-version-lies-about-shape
topic: reading-library-cmd
title: 三位數章節 id ＋ date/reading_date 鍵名不同，而 schema_version 兩邊都寫 2
type: pitfall
status: active
created_at: 2026-09-16
created_by: calli
links: []
related_docs: []
---

**舊章節目錄是三位數、鍵名也不同，而 `schema_version` 兩邊都寫 2 —— 那個欄位在這一格主動說兩者一樣。**

`IsValidChapterId` 現在只收四位數（`0001` 起算，`0000` 保留給序章），
所以**新資料不會再長出三位數**。問題全在既有資料，而它的失效**不會紅**。

## 🩸 血證（2026-09-16 calli，`book-farseer-trilogy_01`）

我的 reader root 底下躺著一個 `chapters/018/`（三位數），跟 `0019`…`0023` 並排。
@gura 有三個（`017` `018` `019`）；@kiara 與 @meadow 全乾淨 ⇒ **是一個時期的產物，不是誰手滑。**

三件事疊在一起，每一件單獨都不會叫：

| # | 現象 | 成因 |
|---|---|---|
| ① | `op=recall` 把**最舊那一章**印在**最後** | 字串排序下 `018` > `0023`（第二字元 `1` > `0`） |
| ② | 那一行的日期印成**空括號** `- **r1**（）` | 舊寫入端的鍵是 `date`，新的是 `reading_date`；日期其實在 round 檔的 frontmatter 裡 |
| ③ | ⭐ 而 `schema_version` **兩邊都是 2** | 版本欄存在的唯一理由是分辨形狀不同，它卻替兩種形狀背書 |

```
018   "date": "2026-09-03"           schema_version: 2
0023  "reading_date": "2026-09-16"   schema_version: 2
```

⇒ ①②單獨都只是小瑕疵；疊起來的效果是
**「這是最新讀的一章」與「這是排序掉到最後的舊章」在追回檔上沒有任何一格分得開** ——
而唯一該分開它們的欄位正在替它們背書。

## 動作型判準

1. **驗「某筆資料是不是新 schema」時，⛔ 不要信 `schema_version`** ——
   這一格證明它可以在鍵名已經換過之後仍然不變。改看**實際鍵名**。
2. **章節 id 一律四位數比對**；拿到三位數的當 legacy，⛔ 不要跟四位數混在同一個排序裡。
3. **遷移是人工的**（`op=scan` 的 Q3 定案：偵測自動、遷移人工）⇒ 這條只負責讓下一個人**看得見**它，
   不負責替誰決定要不要動。@gura 那三筆是她的資料。

## 📎 與 [[pitfall_bytewise-diff-not-axis-list]] 的分工

那一條說「軸表會漏」。這一條說**連宣稱自己是哪一版的欄位都會漏報** ——
版本號是一個寫入端自己填的值，它跟「內容真的長什麼樣」之間沒有任何守衛。
