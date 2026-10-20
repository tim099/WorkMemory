---
id: knowhow_special-building-landmark
topic: town-replica-from-plans
title: 地標走特殊建築；大型建築實心再挖殼的雕法
type: knowhow
status: active
created_at: 2026-10-20
created_by: kaguya
links: []
related_docs: []
---

一棟只在史實那一點出現的建築（加拉塔塔、聖索菲亞）⇒ 特殊建築，不是「地塊點名模型」（Tim 10-20 糾正過一次）：world/town.js SPECIAL 表一種＋城鎮那一筆的 special；模型 meta use ＝ 那一種；tools/townmodels.py MODELS 同名（模型住 Blocksail world/townkit/，Blockside 不用重建）。

- 大型建築的雕法（聖索菲亞，0.4 m／格）：參數全寫進 plan.json（provenance 分參考資料／設計決定）⇒ numpy 先把量體當實心 ⇒ 只留離外面的空氣 1.2 m 內的格（distance_transform_cdt）⇒ 窗與門 ＝ 最外一格挖掉、第二格塗暗色。53 萬格、10 秒產完。
- 地塊 ＝ 作品框；地塊中心算框的中心（不是穹頂中心），rot ＝ 正面朝向（從東順時針 ＝ 方位 − 90）。
- 城鎮那一筆是 JSON 一行：⛔ 整行 json 重存會改寫手寫的數字（41.0010 → 41.001），只在行尾插欄位。
- 坑：窗的遮罩只限一個座標 ⇒ 刻進旁邊扶壁的斜頂；圖紙的方塊參數順序傳錯 ⇒ 一整片色塊（先看圖再交）。
- 讀數要報：三角形數（聖索菲亞近 27 萬）、遊戲裡的 fps 與有沒有對照組。
