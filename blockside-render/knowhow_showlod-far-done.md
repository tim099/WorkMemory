---
id: knowhow_showlod-far-done
topic: blockside-render
title: 表演船遠那一級做完了（TASK-1104）：八方向看板的接點、烘頁的布與陰影坑
type: knowhow
status: active
created_at: 2026-11-01
created_by: erina
links: [blockside-render/knowhow_showlod-mid-done]
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/60_showlod.js|D:/Unity/Blocksail/external/Blockside/src/SHOW_SPEC.md]
---

**表演船遠那一級做完了（TASK-1104；Blockside dev d5ad68c／349939a）：交接帶、再調貼圖時看這裡**（erina 2026-11-01）

**程式**

js/60_showlod.js：
- showlodPrep 給 lv 2 的條件：
  - 距離 ≥ L.f × lk
  - 型有 im
  - RBAT.cull 開著
  - 著色器 glProgTry 好了

  少一項 ⇒ 照中那一級畫，記在 farMid。
- showlodFarSet：照船現在的矩陣，算 8 個方向在世界的水平方向，取點積最大的那個；寫進 B[o·12]。
- showlodFarDraw：一艘一個 drawArrays，在主畫面帆之後、反射船殼之後各畫一次。

整艘不畫的接點（都照 SHIPS 序號判 SHOWLOD.lv[o] === 2）：
- rbatShip／rbatCheck：開頭 return
- viewVis：v = 0
- csmOwners：cst 0
- gpuSlots：不給
- drawCrew：跳過

所以 showlodPrep 挪到了 frameBegin 的 gpuSlots 之前。

**烘**（showlodShots，同 ?showlodbake 頁）
- 船擺正：矩陣只留位置 × shipBaseMatrix。
- 正交：鏡頭在船格子中心的垂直軸往外 φk，往裡看。
- 要關掉的東西：
  - 霧：用 WX.fog，⛔ 不寫 CAM.fogK（owner_check 擋 CAM，只有 03_camera.js 能寫）
  - LAMPS.n
  - 陰影：PASS.sh 0
- 陽光 1：WX.sun、TOD_SUNK。
- 拍完的處理：
  1. 2 倍平均
  2. 十六張一起裁，左右對軸對稱
  3. 透明的像素 bleed 四圈
- 看板的右手邊 (dw.z, 0, −dw.x)：烘頁第一張驗它在畫面右邊，不是就整張左右翻（stat.flip；目前都是 0）。
- 每公尺 2 ÷ lk 像素，取到 0.25，最少 0.5 ⇒ 30 型共 0.95 MB 貼圖。一律 2 的話是 3.5 MB。

**會咬人**
- 烘頁沒跑模擬 ⇒ 布 c.hidden 一直是 true，戰鬥帆整排沒帆。
  - 照 30_rig_physics.js physVis：clothInit(c)、clothBrace(sh, c)、hidden = false。
  - 收的那一套要 hidden = true。
  - 拍完 p、hidden 放回去。
  - 跟 sailk（GPU 算帆）無關：?sailk=0 一樣沒帆，先排除過。
- 頁面還沒畫過一格 ⇒ SHM 是 null，setCommon 讀 SHM.mats 就崩。崩不崩看頁面先跑了幾格：第一次全部烘，30 型壞了 11 型。
  - 先叫 shadowMaps()。
  - ⛔ 不能叫 render()：CAM.vp 可能還沒算，reflPass 的 m4Into 讀 null。
- 300 m 硬切遠那一級的特寫：糊、偏灰（船頭對鏡頭時多半是桅杆、繩）。那是給 800 m × lk 外用的，不能拿特寫判好壞。
  - 判法：真正的距離截兩張，配同一頁兩張的對照組。
- 量測時背景在烘（無頭 Chrome）⇒ 遊戲裡的 ms 跳一倍。A/B 要等別的 Chrome 停了再量。

**量具**：session 55fb05f7 scratchpad m1104/
- farmeas.js：關／中／遠三種各數一格 draw、三角形。
- farab3.js：三種輪流 4 輪，rprof(20)。
- farshot.sh：同一機位截中、遠兩張。
- bake_try.py：直接跑烘頁，把貼圖畫成 PNG。
- atlas_png.py：showlod.js 裡某一型的貼圖 ⇒ PNG。
