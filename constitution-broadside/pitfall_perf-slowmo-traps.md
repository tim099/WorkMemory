---
id: pitfall_perf-slowmo-traps
topic: constitution-broadside
title: 卡頓與殉爆慢動作的坑（從外面計時猜錯、斷船太早、水花不能照畫面走、量具）
type: pitfall
status: active
created_at: 2026-10-13
created_by: ame
links: [pitfall_shreds-rain-traps]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13 晚上（TASK-0531 卡頓與殉爆慢動作、TASK-0535 進水 %）踩到的：

- **卡頓要逐幀分段量，不要從外面整段計時猜**：G.run(1/60) 一次 54 ms 看不出是誰；`?debug=1` 的 `__game.live(秒)`（同步呼叫真的 frame()，dt 照真實時間 ⇒ 追趕迴圈補幾步跟真的玩一樣；預覽分頁在背景時 rAF 停也量得到）＋ `__game.frames({ top, sum })`。開戰第一輪對砲 240–320 ms 的那幾格，205–265 ms 是砲彈（stepBall 每 5 cm 一小段、一步 150 段 × 每段掃全船的繩與人）。
- **砲彈的省法要判定一個都不變**：每一步先挑「這一步飛得到的範圍」（ballNear），順序照原本的；模型不另列、蓋戳記讓 hitModel 照原本的桶跳過（⛔ 改走挑出來的清單 ⇒ 兩個模型疊在一起時先打到哪個會變）；跟著帆桁轉的用圓柱挑。驗：30 秒兩種子＋420 秒三種子（HEAD 對新版）逐字相同。
- **彈坑分步挖時，斷船那一刀要等彈坑挖完**：殉爆那一刻就切 ⇒ 還沒挖到的彈坑格被連通判定一起搬進斷開的半截（27 萬格、上面沒有彈坑）。
- **船邊水花的「生」不能照畫面的時間走**：wpShip 拿這一步與上一步的船矩陣算速度，慢動作時改成每格畫面 ⇒ 沒走模擬的那幾格速度 0、走了的那一格除以很小的 dt 暴衝。只把「飛與散」（stepWater(dt, 'fly')）搬到畫面那邊。
- **慢動作不改戰局的驗法**：同一個種子殉爆後 ① 照畫面迴圈跑、慢動作開 ② 慢動作關 ③ 完全不經過畫面迴圈（G.run），三種在同一個模擬秒數比讀數（60748a03 ×3）。
- **frame(ts) 的參數叫 ts**：在裡面再宣告 var ts（時間倍率）會蓋掉時間戳 ⇒ 改叫 sk。
- **航速也用下沉 ÷ 9 m**（31_sailing.js）：改 HUD 的進水 % 時只改顯示那兩處，動了航速就改戰局。
