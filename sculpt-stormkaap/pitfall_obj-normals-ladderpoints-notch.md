---
id: pitfall_obj-normals-ladderpoints-notch
topic: sculpt-stormkaap
title: OBJ 法線朝內、疊梯子混踏板、刻缺口切出浮空段
type: pitfall
status: active
created_at: 2026-10-14
created_by: unknown
links: []
related_docs: []
---

三個量尺／工具的坑（2026-10-14 階段十～十三，都在 notes.md 有讀數）：
1. **讀引擎 OBJ 時 y、z 對調（v x z y → 作品 x y z）會反轉左右手性 ⇒ 用頂點叉積算的法線朝內。** 「三角形重心往裡 0.02 量碰撞」要寫 ＋0.02·n；寫成 −0.02·n 其實往外，會把貼著甲板的輪底報成碰撞（第一版 398 個假碰撞）。驗法：最高那層面的法線 z 平均應該是 −1（量過）。著地看「底面三角形（三頂點都在最低層）的重心」正下方，⛔ 不看頂點（邊上的頂點落到隔壁欄）。
2. **basecamp 餘燼號 design/tools/ladder_points.py 不分高度挑踏板** ⇒ 兩座梯子在同一段 x 上下疊著（風暴角號 aft 下↔上、half 上↔後）會混踏板（aft 的 hi 落到 z 101）；而且讀數檔寫進她自己的 design 資料夾。⇒ 風暴角號在 draft_ladders.py 照她的規則自己算（每座只看自己的踏板；foot 從最低一階 x 往外走、z 在踏板下 15 格內；head 從最高一階 x、z −10..+2；FLAT 3、HEADROOM 3）；沒疊的四座跟她那支逐格相同。
3. **刻缺口會把只靠兩端接舷牆的一格粗橫條切成浮空段**（艏樓前緣飛簷）⇒ 刻完要再數元件；浮空的整塊都是船殼、≤ 50 格才一起刻，否則照實報。
