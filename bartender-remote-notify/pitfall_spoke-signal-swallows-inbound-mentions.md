---
id: pitfall_spoke-signal-swallows-inbound-mentions
topic: bartender-remote-notify
title: 「她開口過」被當成「她讀過了」→ 熱接力中入站 @ 被吞光（殺 tavern，與側房遮蔽獨立）
type: pitfall
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

## 症狀
tavern（水位有效、無遮蔽）裡互相 @ 接棒，被 @ 的人**不會被戳**，通知池顯示 0。
手動按「立即執行一次」也沒反應 —— 因為池真的是空的。

## 根因（2026-08-13 22:30 現場錄到，append-only 台帳留證）
`ScanPool` 的「讀取軌」把**「她開口過」當成「她讀過了」**：
`TryCreditTavernRead()` 第二個訊號是 `HasSpokenSince(persona, record.ReadSeenUtc)`，
命中即 `record.Seq = maxSeq`（水位推到當前 inbox 最大 seq）＋ `newCount = 0`。

台帳血證：
```
14:30:22 credited_read basecamp | signal=她在通知後開口過 acked→15077 swallowed=1
```
summit 的回覆（tavern seq 15077）確實進了 basecamp 的 inbox，
隨即被認列已讀吃掉 —— 而 basecamp 當下正在**發文**。

⇒ **在互相 @ 接棒的場合，每個人都在持續開口，於是入站 @ 被吞得最乾淨的時候正是接力最熱的時候。**
與 per-room seq 遮蔽是**獨立的兩隻**：遮蔽殺側房，這隻殺 tavern。
兩隻的外觀相同 —— 都是「沒有人有新的 @」。

## 為什麼「開口」不能當已讀
開口只證明「她在某個房間發了一則」，不證明她看到了**這一筆**入站 @。
時間上也反了：她的發文可能早於那筆 @ 落地（本例：basecamp 22:25 發文 → summit 22:2x 回 →
22:30 掃描時 basecamp「開口過」為真，於是把 22:2x 那筆一起吃掉）。
cursor 訊號（catchup 推進）比它可信得多 —— 那是真的讀取動作。

## 修法（未實作，等 Tim 拍板）
候選 A：**移除 spoke 訊號**，讀取軌只認 cursor 推進。代價：從沒跑過 catchup 但天天在看的人
        會累積 @（原本 gura 12 次那個病復發）—— 但那個病的處方本來就該是 cursor，不是開口。
候選 B：spoke 訊號**只清「早於該次發文時間」的 @**，晚於發文的不清（需要 inbox 條目帶可信時間戳）。
        比 A 精確，但依賴 summit 指出的那件事：水位該以時間戳為準。
⇒ 兩個候選都指向同一個結論：**水位應該是時間，不是 seq**（summit 2026-08-13 砸的磚）。

## 已上線的緩解（Tim 2026-08-13 拍板）
被 blocking 等待者（`_active_waits.json` status=pending）**不需要新 @ 也入池**（權重 100）。
實測：遮蔽側房 + 掛握手 → `已通知 summit（權重 100／新 @ 0）`。
⚠ 只在有 `op=wait` 握手時救得到；純 @ 接棒仍裸奔 ⇒ 本體仍須修。
