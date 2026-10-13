---
id: pitfall_sea-gfx-traps
topic: constitution-broadside
title: 海浪與畫質管線的坑（彈簧週期、bob 的意思、重複紋路、VoxelKit 著色器、陰影取樣器、MSAA、量效能）
type: pitfall
status: active
created_at: 2026-10-13
created_by: unknown
links: [constitution-broadside/pitfall_clew-tops-compass-stall]
related_docs: []
---

2026-10-13 午後（TASK-0522 海浪＋畫質管線）踩到的：

- **浪的彈簧別照船的固有週期**：起伏設 5 秒（巡防艦的量級）⇒ 跟主浪（7 秒）太近、落後 60°，船身對水面的上下跟浪一樣大（0.33 對 0.32 m），看起來像沒在跟。改 2.5 秒（起伏、縱搖）、橫搖 5 秒。量法：船頭／船尾／兩舷設計水線那一格的世界高度 − 當地 seaH，取標準差，跟浪本身的標準差比。
- **sh.bob 的意思變了**：現在是「船身相對船底平均水面（sh.wl）的起伏」；船的矩陣 ＝ −sink ＋ lift ＋ wl ＋ bob。進水、瞄水線那些「水線在哪一層」用 sink − lift − bob（相對的）照舊。⛔ 把 bob 當絕對高度用 ⇒ 水線算錯一個浪高。
- **重複紋路**：四道整齊的浪＋固定方向的漣漪 ⇒ 海面一格一格（Tim 截圖）。解法：波長不成整數比、方向左右交錯、細浪用雜湊亂撒、照 fwidth（像素在海面上多寬）淡掉。
- **體素著色器在 VoxelKit**（別的作品也用）：遊戲裡用 voxFS 換字串（水下顏色照浪、陰影、uClip）；VoxelKit 改了那幾段字串 ⇒ 主控台警告、照原本的畫。⛔ 不要直接改 VoxelKit。
- **sampler2DShadow 一定要綁對型別的貼圖**：陰影關掉也綁 1×1 的比較模式深度貼圖（SHM），⛔ 沒綁 ⇒ 整個 draw 失敗。單元配置：0 fx、1／2 海面遮罩、3 場景深度、4／5 陰影圖、6 反射、7 場景顏色。
- **MSAA framebuffer 不能縮放 blit**：多重取樣的來源只能 blit 到同尺寸 ⇒ 先解析到 RT.B，再縮放貼到畫布。畫布本身 antialias 關。
- **帆的緩衝一格只做一次**（prepRig），在陰影、反射、場景之前；⛔ 每一趟各做 ⇒ 布料緩衝重算三次。
- **量效能**：預覽窗格在背景時 rAF 停 ⇒ 不看 FPS，改量「每格繪製的毫秒數」：G.run(0) ＋ drawImage 把畫布讀回 1×1 逼 GPU 做完；同一場（種子 1797、觀戰 20 秒）。舊版拿 git show 出來的成品暫放同一個伺服器比。resize_window 1600×900 量大一點的畫面。
- **反向對照怎麼做才算數**：G.run(0.1) × 300（每 0.1 秒畫一格），不是 G.run(30)（只畫最後一格，抓不到畫面漏進模擬）。
