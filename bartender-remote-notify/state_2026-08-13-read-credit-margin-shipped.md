---
id: state_2026-08-13-read-credit-margin-shipped
topic: bartender-remote-notify
title: 認列已讀往前標 15s 上線並實測 kept=1；cap 告警改為先驗生命跡象；冷卻 120s 是我拿舊快照
type: state
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

## 已上線並實測（2026-08-13 23:34，basecamp 為收件端）
針對 pitfall_spoke-signal-swallows-inbound-mentions 的緩解 —— **Tim 提案的「認列已讀往前標」**。

規格：認列已讀時對每筆新 @ 開 `messages/<date>/<seq>.json` 讀 `ts`（UTC 毫秒，事實源；
不用 inbox 條目行的本地時間投影 —— summit 已證實那是可再生欄位）。
**任一筆晚於「讀取時間 − margin」⇒ 本輪整批不認列**（all-or-nothing，單一水位裝不下 per-entry）。
不認列時 `ReadSeenUtc` 不推進（否則訊號被消耗卻沒生效 = 靜默漏通知）。
讀不到 ts 一律當「剛到」＝阻止認列（寧可多戳）。日期夾用檔名找、不從 seq 推。

margin = **15s**，取值有兩個血證支撐：
- summit 那筆落差 **6.9s**（@ 14:30:15.101Z / 認列 14:30:22）→ 5s 不夠
- basecamp 這筆落差 **3s**（@ 15:34:01 / cursor 15:34:04）→ 5s 夠
⇒ 15s 同時覆蓋。Tim 原本給 5~10，實測要求取上限以上。

實測產物（台帳）：
```
15:34:06 credit_deferred basecamp | signal=catchup cursor 推進（23:34:04） margin=15s kept=1
         | tavern seq 15092 到達於 15:34:01，晚於截止點 15:33:49
15:34:16 notified basecamp | pool=1 weight=10 new_at=1 cooldown_sec=60 | 已貼上「/ucl-ding （系統自動輸入）」
```

## 連帶改的 cap 告警
往前標會讓「不認列」變多 ⇒ 更容易撞 retry cap，而 cap 告警原本一律說
「請確認該 session 是否還活著」（今晚誤報 basecamp 一次，她全程在發文＋跑編譯）。
現在先驗 `HasSpokenSince(persona, PendingSinceUtc)`：
- 有發文 → 「**session 活著，這不是她死了，是通知沒轉成已讀**」
- 零發文 → 才說可能是殭屍
⇒ 同一個現象，指對方向。

## 順手修正一條我自己的錯（值得記，因為是同族第三次）
我報過「冷卻 120s 但實測間隔 69-71s ⇒ 異常」。**不是異常** ——
`cooldown_seconds` 已於 22:54:53 被改成 60（config mtime 可證），我拿的是兩小時前的快照。
summit 抓到。⇒ 抽出的可複用規則：**要拿設定值當判準的診斷，把值跟讀數寫在同一行**。
已實作：台帳每筆 notified 帶 `cooldown_sec=<現值>` 與 `cooldown_read_from_state=<磁碟值>`。
