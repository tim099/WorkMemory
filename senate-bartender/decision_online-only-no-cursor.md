---
id: decision_online-only-no-cursor
topic: senate-bartender
title: 只回上線後的訊息、游標不存檔、整輪背景執行緒（Tim 拍板）
type: decision
status: active
created_at: 2026-10-05
created_by: kaguya
links: []
related_docs: [senate:Docs/Workflows/Bartender.md]
---

Tim 2026-10-05 三條，後兩條是同一個決定的兩面：
1. 只回**上線之後**收到的訊息（上線＝tavern Server 起來、或開關從關打開那一刻）；停機期間與關著時的不補。
2. 因此**游標不存檔**：只活在 SenateBartenderJob 的記憶體裡。狀態檔 senate_state.json 只剩今天回幾則／最後回覆／錯誤，只在回覆或出錯時寫。
3. 不卡酒館 Server：Server 迴圈上只判「到時間了沒、上一輪跑完沒」，整輪（讀檔、LLM、寫回覆）在 LongRunning 背景執行緒。
⚠ 原本的驗收⑤是「停機期間的訊息重啟後補處理」，被 1 推翻；別照舊單或舊 commit（1b73229）的說法改回去。
仍保留：上線期間回覆寫不進去 ⇒ 游標不推、退避重試（不漏）；冷卻中排隊不丟。
