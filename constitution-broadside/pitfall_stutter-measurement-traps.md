---
id: pitfall_stutter-measurement-traps
topic: constitution-broadside
title: 量卡頓的三個假象：拿掉上限、取樣器開始、城鎮模式的海域窗不跟鏡頭
type: pitfall
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

量「卡頓」的三個假象（TASK-0905，2026-10-25；量具在 session b032dc72 scratchpad crop/）：
1. 拿掉 60 fps 上限（--disable-gpu-vsync）時 GPU 佇列一直滿 ⇒ 卡在任何一支 GL 呼叫，長格的歸因全錯；掉幀照遊戲平常的 60 fps 上限量。
2. CDP Profiler.start 那一刻自己卡一格 500～1,300 ms ⇒ 數長格用不開取樣器的那一趟。
3. 城鎮模式的海域窗跟著船、不跟鏡頭 ⇒ 載完之後鏡頭怎麼動都沒有新塊；君堡的卡是進城後前 20 秒（地球儀的拉近動畫蓋著載入）。
另：拿掉上限長時間跑的那一段，機器當機過一次（因果沒確認）。
另：我自己的靜態伺服器＋新設定檔量到皇家港城底下是海，Tim 的遊戲裡是地 ⇒ 那個環境的地形讀數別當事實（未解釋）。
