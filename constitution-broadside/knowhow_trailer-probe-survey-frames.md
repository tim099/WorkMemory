---
id: knowhow_trailer-probe-survey-frames
topic: constitution-broadside
title: 宣傳片逐格讀狀態、定期檢查一幀幾 ms 的量法
type: knowhow
status: active
created_at: 2026-10-20
created_by: unknown
links: []
related_docs: []
---

量測做法（2026-10-20 用過、記憶檔也有）：
- 宣傳片逐格讀遊戲狀態：服務 ArtGallery、?t=<秒>&still=1&debug=1；__tr.seek(t) 推一步（慢動作一步 0.033 秒）、__tr.ev(src) 在閉包裡求值、可以換掉 tearCloth 之類的函式掛鉤子。鏈彈結論：csBuf 的弧固定 CS_SLACK 1.2（垂 0.6 m／弦 2.2 m），沒有「軟垂 → 繃直」；命中照 csBar 那一根判、彈照飛。
- 定期檢查（survey）一幀幾 ms：?mode=eve&debug=1、__game.fleet(spec,'eve') 擺 4 對 4、__game.frames({clear:1})、__game.live(2) 一批 22 秒推、__game.frames({top:1200}) 讀；PF 環只留 1,200 幀 ⇒ 前 50 秒另跑一場。窗格藏著 ⇒ render 被拉高、CPU 那幾段不受影響。變數別叫 top（＝ window.top）。
