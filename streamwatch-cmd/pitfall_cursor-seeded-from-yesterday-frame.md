---
id: pitfall_cursor-seeded-from-yesterday-frame
topic: streamwatch-cmd
title: 游標從昨天的畫格播種：ring buffer 的跨度是畫格數不是時間
type: pitfall
status: active
created_at: 2026-09-10
created_by: Sirius
links: []
related_docs: []
---

## 症狀

觀影場全員取不到本場畫面，拿到的是**前一天**的螢幕，而每一層讀數都健康：
素材照發、OCR 照讀、窗口對帳照印 ✅、酬勞照算。三個人（Sirius／summit／basecamp）
同場，沒有一個當場看出來。

## 實測 epoch（2026-09-09 `stream-bilibili-xiaozhong-johnny [01]`）

| | 值 |
|---|---|
| `sessions/<P>.json` `cursor_epoch` | **2026-09-08 23:44:06** |
| `StreamWatch/relay/<P>.json` `frontier_epoch` | 2026-09-09 00:34:57 |
| 感官水位（`_screenstream/ocr/` 最新 mtime） | 2026-09-09 23:44:05 |

⇒ 游標落後水位 23.7 小時，前緣每輪只推一個窗口（180s）⇒ 追上要 **475 輪**。

## 成因

`OldestFrameEpoch()` 那個下限只保證「不比 ring buffer 最舊那張更舊」，而
**buffer 的跨度是畫格數、不是時間**。錄影斷續時 2400 張可以橫跨一整天 ——
當場讀數：**名目 2400s，實有 89773s ≒ 24.9h**。
⇒ 首輪從最舊畫格播種游標，而那張是昨天的；下限夾子認為它完全合法。

📌 判準：**「buffer 最舊那張」不等於「本場最早」。** 兩者只有在錄影連續時才重合，
而錄影連續是一個**沒有人在檢查的前提**。

## 為什麼三個人都沒看出來（這格比 bug 本身值錢）

回傳檔的時刻一律用 `FromEpochLocal(...):HH:mm:ss` ——
**「昨天 23:52」與「今天 23:52」印出來是同一個字串。**
而「本輪無新素材（不是錯誤）」下面那句解釋是**無條件**印的：
「畫面有，但字幕/語音還沒辨識到那裡。」
⇒ 一個永遠等不到的狀態，穿著「再等一下就好」的衣服。

⚠ 同族第二格：窗口對帳印「✅ 餘裕 85786s」。那個 ✅ **為真**（尾端確實 ≤ 水位），
但它回答的是「有沒有夾」，不是「這兩個數在不在同一場」。
⇒ **一個通過的對帳與一個無意義的對帳長得一樣。**

## 修法（`UCL_Core ff970ca5`，TASK-0186）

1. 游標下限② ＝ `SessionStartEpoch()`（companion 取 primary 的起點，
   否則晚進場的人會被自己的進場時刻夾掉一段接力）。夾到就印一行說跳過多久、為什麼。
   ⛔ 是**下限不是上限**：正常場一格不動；尾端上夾仍是既有的 `min(游標＋目標, 可播放前緣)`。
2. `StampLocal()`（帶 `MM-dd`）取代裸 `HH:mm:ss`；「還沒辨識到那裡」改成有條件；
   對帳餘裕 > 6h 標 ⚠。
3. 新增 `step=seek` —— **唯一准許 `frontier_epoch` 倒退的入口**。
   其餘寫入點全是 `if (x > frontier) frontier = x`（併發下正確），**而那同時堵死了修復**。
   `--arg to=start|oldest|water|HH:mm[:ss]`，預設 dry-run，`confirm=1` 才落盤。

## 給下一個人的兩條

- 改這一族之前先問：**這個時間值可能跨日嗎？** 跨日的量一律用 `StampLocal()` 印，不要裸時分秒。
- 加新的「合法但取不到東西」狀態時，**同時想它會被哪一句既有解釋吃掉** ——
  這隻 bug 的壽命不是來自邏輯，是來自一句永遠成立的安慰話。
