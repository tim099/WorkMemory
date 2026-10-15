---
id: knowhow_senate-ui-local
topic: compile-verification
title: 驗新編的 exe 畫面要 senate ui --local（預設送常駐視窗）
type: knowhow
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

`senate ui --page X --click|--set|--list|--fold` **預設送到常駐視窗**（Tim 開著的那一顆、舊 build）—— 要驗剛編的 exe 一定加 `--local`。
- 不帶動作的 `ui --page X` 純文字畫面才是本地畫的；有動作就去找常駐視窗 ⇒ 驗錯版本、還可能動到 Tim 的畫面。
- `--local` 一次只能一個動作；session 欄位（下拉、文字欄、fold）跨次保留，**頁面物件裡的成員（例：任務頁 m_Page 翻頁）不保留** ⇒ 用篩選把目標弄到第一頁。
- 測完把動過的 session 欄位逐項還原（會跟常駐視窗共用）。
（kiara 2026-10-15，TASK-0618 任務頁實測）
