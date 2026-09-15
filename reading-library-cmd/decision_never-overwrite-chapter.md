---
id: decision_never-overwrite-chapter
topic: reading-library-cmd
title: 章檔永不覆蓋（出 v2）＋ 重出前核對 seq 身分（TASK-0152／0217）
type: decision
status: active
created_at: 2026-09-15
created_by: basecamp
links: []
related_docs: []
---

**Tim 2026-09-15 拍板**：`WriteChapter` **無論如何都不覆蓋既有章**。
既有 `NNN.txt` 一個位元組都不動，本次輸出落到 `NNN_v2.txt`（再來 `_v3`…）。

**配套三格**：
- `_vN` **不符合 `^\d{3}$`** ⇒ `ResolveChapter` / `FindOverlaps` / `???.txt` 列舉都看不到它。**版本不是章。**
- `iForce` 語意收窄（⛔ 不再是「覆寫」）：`false` ⇒ 既有章存在就拒絕；`true` ⇒ 允許重出而落在新版本上。
  ⚠ **參數名現在比行為大** —— 改名要跨兩個 repo 的呼叫端，未做。
- 落檔前一道**最後防線**：走到寫檔時目標若仍存在 ⇒ 拒絕並報「程式錯誤」。
  ⛔ 它防的不是二十行前那段，是**未來有人在中間插一條路**。
- 同族順手修：`SCP_BookStore.CountProse` 用 `*.txt` 數章數 ⇒ 會把第二版算成多一章，改 `???.txt`。

**接著量到的更大一格（TASK-0217）**：章檔表頭只記 `seq A – B`，**沒記是哪一區的 seq**，
而酒館 seq 隨區域分岔 ⇒ 重出舊章會產出**格式完整、seq 連續、回讀驗證全過**而內容是別批訊息的章。
逐章盤：有實錄段的 **39** 章裡本文對得上 **0**、對不上 **27**、seq 現在不存在 **12**。
⇒ `VerifyChapterIdentity`（比**本文**不比時間）接在「既有檔存在」那一格、排在 `iForce` **之前**；
對不上 ⇒ **整個拒絕，連 `_vN` 都不出**。

⚠ **未做（明知）**：沒有替章檔補區域定語 ⇒ 那 39 章仍然重出不了，**而那是對的**：
它們的號碼在這個訊息庫裡沒有意義。要恢復得有人決定「在本區重新匯出成新的一章」。
