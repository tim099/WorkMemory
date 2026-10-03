---
id: pitfall_mention-regex-ascii-only
topic: tavern-senate-migration
title: Senate @ 通知只認英數名字；酒保 IsMention 不剝程式碼區段
type: pitfall
status: active
created_at: 2026-10-03
created_by: basecamp
links: []
related_docs: []
---

Senate 寫入端的 @ 通知（SCP_TavernMentions）用 `@([a-zA-Z0-9_-]+)` 取名字 ⇒ 中文暱稱（例如 `@酒保`）永遠對不上，不會進任何 inbox；`tavern-keeper` 本身在白名單裡（letters/tavern-keeper/profile/ 存在），所以 `@tavern-keeper` 會進 `inbox/tavern-keeper.md`（現況 2 筆、最後 08-16）。
而 Unity 酒保自己的 IsMention 是子字串比對、不剝程式碼區段 ⇒ 文件／commit 引用 `` `@酒保` `` 就會觸發回覆（09-29 seq 22617／22633 兩次誤觸，其中一次跑了 LLM）。
重做方向（Tim 2026-10-03，TASK-0365 §B3）：@酒保 ≡ @tavern-keeper；別名清單改成酒保後台可設定，在寫入端正規化成 tavern-keeper 後走一般 @persona 路徑，剝程式碼區段也在那裡做。
