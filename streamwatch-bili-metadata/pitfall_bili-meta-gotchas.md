---
id: pitfall_bili-meta-gotchas
topic: streamwatch-bili-metadata
title: 短連結那條路沒讀數（刻意不驗）／探針容易自帶答案／pubdate 要手算
type: pitfall
status: active
created_at: 2026-08-27
created_by: basecamp
links: []
related_docs: [ucl_core:Skills~/ucl-stream-watch/SKILL.md]
---

下次動 `bili_meta.py` 或它的呼叫端之前，這三格會咬人。

## ① 短連結那條路**沒有讀數，是刻意不驗的**

`extract_id` 裡有完整的 redirect 分支（跟 `b23.tv`、處理 av 網址不轉 BV）——
**它從來沒有被真的走過一次。** 原因不是漏掉：Tim 2026-08-27 說「短連結我不會用，不用考慮」，
所以 TASK-0067 把那兩條驗收**劃掉標為不適用**，而不是打勾。

⚠ 所以：**哪天有人貼一條真短連結進來，那是那條 code 的首航。**
別把「單子已 done」讀成「這條路驗過了」——單子上是刪除線，不是勾。

## ② 驗它的時候，探針很容易自帶答案

我 QA 時餵 `https://b23.tv/BV1vM8P6EEDY` 想測短連結，看到「解析出同一個 BV」差點簽過去。
但 `extract_id` 的順序是 **BV regex 先比 → 命中就 return**，
那條字串裡本來就有 BV ⇒ **redirect 分支一行都沒跑**。

⇒ 判準：**測短連結時，輸入字串裡不能出現 BV**（真短連結是 7 碼 hash）。
⭐ 救我的是回傳檔那行「**BV 號來源：直接從輸入字串取得**」——
`summit` 把「怎麼拿到的」印出來，是那次 QA 能自我糾錯的唯一原因。
**動這支工具時不要拿掉那一行**，它是這支工具唯一的自證機制。

## ③ `pubdate` 只印 unix，對帳要手算

現況印 `1787270400（unix）`。QA 時要跟外部證人（B 站頁面顯示 `2026-08-21 08:00:00`）對帳，
我是**手算**才確認一致的。TASK-0067 把「併印人可讀時間」列為建議、**未做且不擋結單**，
條文留在單上。⇒ 下一個人動這裡時，順手補掉比較划算。

## 決策落在哪（不在這裡複製，只指路）

- **工具只取資訊、填資訊仍由主觀影者負責**（Tim 2026-08-26）→ `ucl-stream-watch` SKILL.md「bilibili：先用工具取資訊，再自己填」
- **各集獨立、episode 用目前最大章號 +1**（Tim 2026-08-27）→ 同上「bilibili 的集數怎麼給」
- **短連結不適用**（Tim 2026-08-27）→ TASK-0067 驗收條文（刪除線）
- **髒輸入不擋但必須說**（QA 追加）→ TASK-0067 驗收條文第 9 條
- 量退出碼不要經過 pipe → summit 已入 Lessons（`40cdb0206`），**這裡不重複**

⚠ 本主題是 TASK-0067 的鷹架，單子已 done ⇒ **可歸檔**。
留著只為兩件事：①②③ 那三格咬人點，以及讓 0067 的 `memory_topic` 不是壞鏈。
