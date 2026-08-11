---
id: pitfall_stt-prompt-echo-on-silence
topic: stt-audio-understanding
title: 靜音段 whisper 會吐出 initial_prompt 本體 —— 幻聽的第三種形態
type: pitfall
status: active
created_at: 2026-08-11
created_by: summit
links: []
related_docs: []
---

2026-08-11 陪看《魔法公主》04 開場實證：畫面全段無字幕（OCR 25 幀命中 1），STT 吐出的是 Tim 設在影音管理頁的 stt_prompt 本體（アシタカ、サン、エボシ御前、モロ、ジコ坊…整串人名清單）。@Sirius 獨立在同一分鐘抓到同一筆並建議立即清空 prompt。

這是今晚同一族的第三種形態：
(1) 靜音 → 吐片尾客套語（お疲れ様でした／ご視聴ありがとうございました，一晚 6 次）
(2) 有聲無人聲（純配樂）→ RMS 閘蓋不到
(3) **靜音 → 吐 initial_prompt 自己**

⚠ 這一筆會通過任何「內容看起來合理」的檢查 —— 全是真人名、真專名。下游拿去做語者比對或情緒判讀會餵出一份完全乾淨、完全假的資料。

**可執行的辨識法（不需要模型）**：輸出與 stt_prompt 高相似 → 直接判為幻聽丟棄，字串比對就夠。

**修法與代價（Tim 已執行清空 prompt，同晚 Part 5 實測）**：
- ✅ 靜音吐 prompt 的形態消失，STT 開始對得上台詞（くるな!人間なんかが嫌いだ! 對上「別過來，我討厭世人」）
- ⚠ **但同一輪「狼」被咬成 ヤマイム** —— 拿掉偏置＝少了幻聽素材，也少了專名的咬字支撐。
**這是取捨不是純勝**，兩邊都要記。
