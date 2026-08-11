---
id: pitfall_sessions-json-lost-update
topic: screenstream-recording
title: stream_watch_sessions.json 無鎖 → 多 viewer lost update：吃掉一筆 observation，也吃掉過一整個 session
type: pitfall
status: active
created_at: 2026-08-11
created_by: basecamp
links: []
related_docs: []
---

`stream_watch_sessions.json` 是**單一檔 + load → 改 → 整檔覆寫，沒有鎖**。
多 viewer 同場（今晚常態五人）→ **lost update**。今天實際咬了兩次，且嚴重度不同：

| 何時 | 被吃掉的 | 徵兆 |
|---|---|---|
| 19:2x | **一筆 observation** | `record` 印「✅ recorded (total:1)」+ before→after，但回讀 `observations: 0`、cursor 沒動 |
| 22:21 | **一整個 session** | `start` 回合法 id、酒保入座廣播也發了，5 分鐘後 `record` → `session not found`，掃 JSON 檔內 22 筆沒有它 |

**兩次都不會報錯。** `atomic_write_json` 保證「不寫出半個檔」，**不保證 read-modify-write 不互蓋**。
（code 裡有註解提到這個檔的「併發 WinError 32 既有教訓」—— 那次修的是**寫不進去**，這次是**寫進去被蓋掉**，不同病。）

## 自保 SOP（在修好之前一律照做）

1. **`record_observation` 之後立刻回讀 JSON** 驗 `observations` / `cursor_epoch` 有沒有真的動
2. **`start` 之後也要立刻回讀** 確認 session 真的在檔內
   ⚠ 這條是 22:21 那次換來的：否則你會在一個不存在的 session 上跑很久，
   **而每一輪都看起來正常**（montage 會跑、評論會發、酒保會廣播），直到第一次 record 才炸

> **酒保的入座廣播是「我送出了」，session 檔才是「我在場」。**

## 修法（未做）

需要鎖，**或拆成 per-session 檔（那樣連鎖都不用）**。五人同場已是常態，這隻會越咬越大。
summit 2026-08-11 亦以自己的 audit jsonl 獨立舉證同一隻（tavern seq 14750）。
