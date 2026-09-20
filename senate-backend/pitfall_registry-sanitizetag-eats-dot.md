---
id: pitfall_registry-sanitizetag-eats-dot
topic: senate-backend
title: registry 的 SanitizeTag 把點改成底線 ⇒ 活著的 Server 被報成 not_running
type: pitfall
status: active
created_at: 2026-09-21
created_by: summit
links: []
related_docs: []
---

🩸 第一版路徑與 registry tag 共用 `.` 當分隔符。實跑之後 `server list` 報 tavern `not_running`，
**而它明明活著、單例鎖也握著**（第二顆起不來就是它擋的）。

成因：`SCP_ProcessRegistry.SanitizeTag` 只留 `[A-Za-z0-9-_]`，**點號被改寫成底線**
⇒ 登記時存 `senate_server_tavern`、查詢時拿 `senate_server.tavern` 去比 ⇒ **永遠對不上**。

⇒ **「它不在」與「我認不出它」同形** —— 而那正是這張單要防的那一族，我自己造了一個。

## 處置

tag 的分隔符改用 `-`（`ServerIds.TagSuffix`），**檔名那邊仍是 `.`**（`ServerIds.Suffix`）。
兩個分隔符不是筆誤，理由寫在 `TagSuffix` 的註解裡。
⛔ 不改 `SanitizeTag`：那是 registry 的地板（tag 會變成檔名），要配合的是呼叫端。

## 📌 抓到它的不是更仔細

是**真的起了兩顆**然後發現讀數自相矛盾（單例鎖擋得住第二顆 ⇒ 第一顆活著；而 list 說沒在跑）。
