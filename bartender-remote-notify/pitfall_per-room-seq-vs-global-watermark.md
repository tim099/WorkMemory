---
id: pitfall_per-room-seq-vs-global-watermark
topic: bartender-remote-notify
title: 通知池永遠 0：seq 是 per-room 編號，已讀水位卻是跨房共用單值 → 側房永久遮蔽
type: pitfall
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

## 症狀
後台「通知池（0）— 最近一次：沒有人有新的 @（通知池是空的）」，
而當下同事**確實在互相 @ 接棒**（Tim 於 TRPG 場次觀測到數次，成因當時未查明）。

## 根因（2026-08-13 basecamp wake#57 量到，非推論）
`seq` 是 **per-room** 編號，已讀水位 `record.Seq` 卻是 **per-persona 單一值、跨房共用**。

三個各自量到的事實：
1. 每房訊息檔各自從 1 起編 —— `rooms/tavern/messages/.../00015075.json`
   vs `rooms/trpg-yachiyo/.../00000109.json` vs `trpg-midnight-relay/.../00000011.json`
2. `remote_notify_state.json` 每個 persona 只有一個 `seq` 欄
3. `UCL_RemoteNotifyService.CountInbox(persona, sinceSeq)` 拿那**一個**水位掃**所有**房的
   `inbox/<persona>.md`，判準是 `seq > sinceSeq`

⇒ 水位一旦被 tavern 推高（現況 6 個 persona 全部 ≥14494），
**所有側房的 @ 永遠算不出「新的」** —— 不是延遲一輪，是永久靜默。
實測：summit 在 trpg-yachiyo 有 5 筆未歸檔 inbox 條目（seq 103-108），
`seq > 15075` 的筆數＝**0**。

## 為什麼查不出來
淘汰發生在入池**之前**，而舊版看板只顯示「池裡有誰」。
六種完全不同的成因（沒在線／沒條目／都已讀／本輪被認列已讀／停戳／冷卻）
壓成同一句「沒有人有新的 @」。那句話沒說謊，它只是把六個答案壓成一個。

## 已做（本次）
逐人判定痕跡 + 逐房 inbox 分解，遮蔽房標 `⚠ 整房遮蔽` 紅字；
`SummarizeScan()` 把六種成因分開報。**行為一行未改** —— 先讓它現身再修。

## 修法（未實作，等 Tim 拍板）
水位改 per-room：`state[persona].rooms[<room>] = seq`。
⚠ 是 state 檔 schema 變更，且要處理既有單值水位的遷移
（把舊的單值當 tavern 房水位、其餘房從 0 起算 —— 不能一律套用，
否則側房會一次湧出上百筆歷史 @ 把人戳爆）。
