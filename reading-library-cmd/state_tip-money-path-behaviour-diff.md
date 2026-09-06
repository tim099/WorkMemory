---
id: state_tip-money-path-behaviour-diff
topic: reading-library-cmd
title: 金流第一支 tip 對拍：欄位逐欄相同、行為相同；字面與檔名時戳不同（實測排序未壞，而沒壞是因為沒撞上）
type: state
status: active
created_at: 2026-09-06
created_by: basecamp
links: []
related_docs: []
---

## 金流第一支 `tip` 對拍：**欄位逐欄相同、行為相同；字面與檔名時戳不同**

**實測 2026-09-06（basecamp）。Tim 授權用 `Template` persona 測金流（該帳戶是測試用），
並指定打賞給 basecamp 的《山腳的營地》當績效獎金。**

⚠ **我是受益人** ⇒ 金額取**能跑通對拍的最小值 1 token**（兩側各一次）。
⛔ 獎金大小不由我決定 —— 那一格留給 Tim。

### 量法

兩側各真跑一次（⛔ 沒有 dry-run，這條路只能真跑）：
```
python library.py tip --book basecamp-foot-of-the-mountain \
      --tipper Template --tipper-persona Template --tokens 1 --no-notify
senate ucmd run Books --persona basecamp --arg op=tip --arg book=… \
      --arg agent=Template --arg persona=Template --arg tokens=1 --arg no_notify=true
```
⚠ C# 那側 **`persona=` 是行為人**，而 `--persona` 旗標也會把值戳進 args ⇒ **必須顯式覆寫**，
否則打賞者會變成呼叫者本人（而那會被「受益人不可是自己」的守衛擋下 —— 守衛救了這一格）。

### 讀數

| 層 | 結果 |
|---|---|
| **欄位** | **逐欄相同**：12 個 key、同順序、同值語意（`beneficiary` 都正確解成 `claude-code`/`basecamp`） |
| **行為** | 相同：debit 真落帳、券 1+1、`voucher_status: issued`、`no_notify` 兩邊都生效 |
| **金流實證** | Template 餘額 **142 → 140**（兩側各燒 1）—— ⛔ 不是讀回報字串，是回讀帳戶 |
| **字面** | **不同**：python 2 空格縮排＋`": "`；C# **tab** 縮排＋`":"`（`SCP_JsonWriter` 預設 tab） |
| **檔名時戳** | **不同位數**：python 12 位（6 位小數）／C# 13 位（7 位小數，.NET ticks） |

### 檔名時戳那一格：**實測沒壞，而它沒壞的理由不是設計**

`_load_tips()` 用 `sorted(glob)` 的**字典序**當時間序。位數不同看起來會排錯 ——
⛔ 我沒有推理，我量了：把 9 筆有時戳的檔名各自解析成真實時間再排一次，
**字典序 == 真時間序 = True**。
（原因：小數靠左補零，逐字元比較與數值比較等價；長短不同只在完全相等時才並列。）

⚠ **順帶量到第三種檔名形狀**：`20260611_kotoko_f0706693.json`（只有日期、**沒有 T 時戳**，3 筆，
T-BOOKS-STORAGE Phase A 從聚合檔遷過來的）。
ASCII 上 `T`(0x54) < `_`(0x5F) ⇒ **同一天**若同時有兩種形狀，date-only 那筆會排到 T 時戳之後。
現況沒有同日混用 ⇒ **沒壞是因為沒撞上，不是因為有人保證過。**

### 判定

`tip` **通過**（資料層與行為層）。與 `tips`／`donations` 同族的結論：
**資料相同、呈現不同** —— ⛔ 不可寫成「一樣」。

### 還沒量的兩支（照實列）

- **`donate`**：會在圖書館**新增一本書** ⇒ 污染面比 tip 大，量法要先想清楚。
- **`publish`**：⛔ **綁 ②-bis** —— 那正是 @gura／@Sirius 兩本書將來要走的那一步，
  在舊 store 去留拍板前不碰。
