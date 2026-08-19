---
id: state_2026-08-19-auto-export
topic: streamwatch-cmd
title: 收工自動匯出上線（BUG-9/10 已修）＋ BUG-11 未 commit
type: state
status: active
created_at: 2026-08-19
created_by: summit
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_StreamWatch_Cmd.md]
---

# 2026-08-19（summit wake#57）—— 收工自動匯出上線 ＋ 隨之而來的第一隻

## 這次做了什麼（已 commit：UCL_Core 003abc6a，單層）

- **BUG-9**：`export-watch` 匯出後回填 `sessions_log.jsonl` 的 `exported_chapter`。
  台帳 append-only ⇒ 走 **append 修訂事件**（`record_type=export`），讀取端取同 session_id 最後一筆。
  程式碼實際落在 calli 的 65beb17c（她 commit 時抓到我編輯中的檔），我的 003abc6a 補文件與跳脫修正。
- **BUG-10**：收工自動匯出。`prepare` 新增 `chapter_title`（填了才啟用）／`export_chapter`／
  `export_work_title`／`auto_export`；收工結算時 **primary** 自動跑
  `library.py export-watch --from-session <id> --force`。
  台帳新增 `library_media_id`（準備檔以閱讀庫 media id 命名、場次記 work slug）。

## ⚠ 未完成（下一個接手的人先看這裡）

- **BUG-11 open，修法在工作區未 commit**：`_resolve_from_session` 把主場與陪同場區間當兩段各掃一次，
  而陪同場區間是主場的真子集 ⇒ 章內訊息重複收錄。修在**呼叫端合併區間**（不是掃描端去重）。
  commit 訊息要帶 `Fixes BUG-11`。
- **主專案指標未 bump**（單層）⇒ 同事 pull 主專案還拿不到自動匯出。

## 拍板與判準（別再重新討論）

- 「章名必須親筆、不給工具代取」= Tim 拍板的邊界；`show_title`（影片標題）**不可**當預設值。
- 章 ≠ 場的處理位置：`--from-session` 併區間（主場 ∪ companions ∪ 已匯進同一章的舊場次），
  且**只有 primary 觸發**（陪同者也觸發的話同一章會被各匯一次）。
- 併章副標、跨章判斷仍是人的事，自動化只覆蓋「一場＝一章」與「同章多場併區間」。

## 血證（這次的形狀）

三隻全部長同一個樣子：**綠燈量的是它自己**。
- 重複章：`收錄 21 筆／未收錄 0／回讀驗證 360 行` —— 21 是含重複的 21，回讀量的是同一份重複內容。
  （calli 的措辭：**回讀驗證被含重複讀數欺騙**。）
- 我的誤診：`tail -3` 看檔尾 ⇒ 宣告「漏了最後兩則」，而檔案排列是「範圍一整段接範圍二整段」。
- 畫布：送 `#9E9E9E` 落盤 `#9191AA`（RGB332 沒有中性灰），而 `placed 10 pixel(s)` 正常印出。

⇒ 下次驗匯出類功能：**逐項對帳（`uniq -c` / 逐 seq）**，不要看筆數、行數、端點。
