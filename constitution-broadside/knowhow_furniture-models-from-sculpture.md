---
id: knowhow_furniture-models-from-sculpture
topic: constitution-broadside
title: 道具換成雕好的模型：furnmodels.py ⇒ 37_furnmodels.js、繞向與置中
type: knowhow
status: active
created_at: 2026-10-19
created_by: unknown
links: []
related_docs: []
---

道具（家具）換成雕好的模型的路（TASK-0569，2026-10-19，dev 32c3bb8）：

- 雕刻端：一件道具一件子作品，掛在一個「目錄作品」上當外部模型，meta `kind=furniture`、`item=<FURN 代號>`；匯出 vxk（include=rig,models,parts,meta）。
- 遊戲端：`python -I src/furnmodels.py <vxk>` ⇒ `src/js/37_furnmodels.js`（FURN_MODEL；⛔ 不要手改），`36_rooms.js furnModelMesh` 有就用、沒有照舊拼方塊。重雕只要重跑這一支再 build。
- 網格是 0.1 m 一單位、相對模型原點（雕刻匯出已乘上放上船的縮放）；比 FURN.size 小 ⇒ 置中。轉 90°（ax='y'）是 x、y 對調 ＝ 鏡射 ⇒ 三角形繞向要跟著對調。
- 繞向慣例：vxk 的模型網格在格子座標裡有號體積是負的（船的矩陣是鏡射，到世界才朝外）；程式拼的 boxMesh 是正的 —— 相反，光照可能是反的，沒動（不在 0569 射程）。
- 家具會被打中（照 meshOcc）⇒ 換模型、改外框都會改戰局：有醫療艙的 6 組對拍換新值。
- 量的時候：GPU 上傳後 T.meshes[i].tri 會清掉，頁面上量三角形數用 `.n`（頂點數 ＝ 三角形 × 3）；閉包裡的函式頁面叫不到 ⇒ 用 node 把那一段原始碼抽出來跑。
