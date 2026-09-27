---
id: pitfall_deferred-outbox-and-glossary-prefix
topic: tavern-senate-migration
title: 延後發文匣先認領再送／沒有 post_seq／詞典顯示前綴要逐字 docs/Glossary
type: pitfall
status: active
created_at: 2026-09-27
created_by: basecamp
links: []
related_docs: []
---

**延後發文匣與詞典路徑的三個坑（2026-09-27）**

1. **認領在送出之前**：`FlushDue` 先改名 `.claimed` 再 Submit、送完刪。當機在中間 ⇒ 那一則留在 `.claimed`、**沒發**；啟動時只列出不重送（它可能已經進了 queue）。⛔ 別改成「送了再認領」—— 那會在當機時發兩則（seq 全域遞增、發文會付錢）。⚠ 這一格只有 selftest 讀數，沒有活體。
2. **延後發文沒有 post_seq**：CLI 回 exit 0＋`scheduled=1`／`deferred_until`。依賴 post_seq 的呼叫端要先看 `scheduled`，⛔ 別把「沒有 seq」讀成失敗去補發。
3. **詞典附註的顯示前綴**：預設詞典根時必須逐字印 `docs/Glossary`（小寫，與 Editor `Cmd_Glossary` 同形 —— 欄位對拍與序列化對拍靠它）；自訂根才印相對／絕對。磁碟上的目錄是 `Docs/Glossary`（大寫 D），描述表 auto 取的是磁碟那個。⚠ `glossaryRoot` 只存 senate.local.json（Tim 拍板），Editor 讀不到 ⇒ 設非預設值時兩邊分岔，遷移追在 TASK-0313。
