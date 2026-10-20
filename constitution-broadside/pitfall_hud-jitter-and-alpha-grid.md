---
id: pitfall_hud-jitter-and-alpha-grid
topic: constitution-broadside
title: HUD 數字抖動與半透明格子疊出網格紋（固定欄寬、模擬層平均、小圖縮放貼上）
type: pitfall
status: active
created_at: 2026-10-18
created_by: trailhead
links: []
related_docs: []
---

Blockside HUD 兩個畫面坑（2026-10-18，trailhead；Tim 截圖抓到）：

1. 數字一行抖（詳細資訊的進水那一塊）：字長不固定 ＋「往外流」有時出現有時不見 ⇒ 整行折來折去。
   解法：每個數字一格固定寬度靠右、tabular-nums（61_hud.js numCell、template.html .nv）、每一段一直都在（沒有就 0）、固定兩行；
   會跳的流量另外在模擬裡取 FL_AVG_T 秒平均（33_flood.js flAvgStep → fl.avg、flRates）。量法：遊戲跑著每 0.25 秒記每格 offsetLeft/Top 與塊高，全部只能一個值。

2. 俯視圖上一格一個半透明 fillRect ⇒ 網格紋：水格（邊長 c 格 × 每格 px）比一個像素還窄，取整數之後有的疊兩次 ⇒ 每 4 個裝置像素一條暗線。
   ⛔ 「多畫半格讓接縫不露底」在半透明時正好製造紋路。解法：每一柱畫成小圖上一個像素（putImageData），整張 drawImage 縮放貼上（61_hud.js gmapFlood）。
   量法：getImageData 讀一塊 12×12 的通道值，有週期就是疊出來的；切到沒有疊層的模式讀同一塊當對照。

另：Tim 要的讀數一律「模擬層記、畫面直接拿」（破口 hWet／hQ、netQ、underN、outN、損壞表 sh.dmg.n、drive.cap）；畫面不自己繞資料數。
