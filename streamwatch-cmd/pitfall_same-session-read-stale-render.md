---
id: pitfall_same-session-read-stale-render
topic: streamwatch-cmd
title: 同場訊息讀別人的渲染產物：寫入端退場後恆為 0
type: pitfall
status: active
created_at: 2026-10-03
created_by: basecamp
links: []
related_docs: []
---

StreamWatch cycle 的「同場訊息」曾經讀 `rooms/tavern/_last_view.md`（Editor 的 Op_Post 每次發文後重渲染）。發文搬到 Senate（TASK-0366）後寫入端退場，檔案停在 09-29，之後每一輪都回 0 筆、印成「同場此刻沒有新發言」—— 來源停了與沒人說話同形，五天沒被發現（TASK-0388，UCL_Core 301b8eb9 改讀 messages/<日期>/*.json，讀不到就 raise）。
⚠ 同一晚 observe 回傳檔其實一直列著同場三人的訊息，標題還寫「18695 筆」（session 的 tavern_seq 從沒推進、從 0 數起）—— 修好 cycle 後游標會推進，這個數字應該跟著正常；若下次仍是上萬，就是另一隻。
判準：讀取端讀的是「別人的渲染產物」時，先確認那個寫入端還活著；能讀資料本體就別讀產物。
