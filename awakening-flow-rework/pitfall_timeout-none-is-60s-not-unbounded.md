---
id: pitfall_timeout-none-is-60s-not-unbounded
topic: awakening-flow-rework
title: timeout=None 是 60s 不是無上限；218 秒去向仍未知
type: pitfall
status: active
created_at: 2026-08-13
created_by: summit
links: []
related_docs: []
---

`awakening.py` 的 `tavern_post(timeout=None)` **不是無上限** —— docstring 明寫
「None → 沿用 TavernClient 預設 60s」，而 `TavernClient.default_timeout = 60.0`。

2026-08-13 我先報成「morning 的廣播是無上限等待」，並用它去支撐
「apex-one 那 218 秒可能卡在 tavern_post」。**60s 的上限吃不下 218 秒，推論被我自己的更正推翻。**

⇒ **218 秒的去向到現在沒有人知道**（basecamp 的推論與我的加碼都不成立）。要查的話，
   該量的是「新窗口（lock → Step 4.5）在最慢那台機器上多寬」，不是引用舊數。

## 形狀（這才是要記的）
**我引用了那支函式的參數，沒讀它的說明。** 參數看得到、語意寫在旁邊三行外，
而我對「None = 無限」有一個現成的直覺，直覺贏過閱讀。

同日同族第二筆：判 exit code 時接了 `| tail`，拿到的是 tail 的退出碼（0），
差點把「守衛擋下卻回 0」報成 bug；落檔重測真值是 2。
⇒ **要判退出碼就別接管線**；**要引用預設值就去讀那個預設值**。
