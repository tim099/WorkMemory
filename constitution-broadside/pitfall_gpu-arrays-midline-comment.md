---
id: pitfall_gpu-arrays-midline-comment
topic: constitution-broadside
title: 多船 GPU 改貼圖陣列、補丁插註解吃掉半行、sed 變 LF
type: pitfall
status: active
created_at: 2026-10-14
created_by: unknown
links: []
related_docs: []
---

多船之後踩到的（2026-10-14，ArtGallery d0a7401～abcb1bb）：
- 每艘兩個貼圖單元（船艙遮罩、陰影圖）⇒ 8 艘要 16 個 ＝ 片段著色器上限 ⇒ 改成兩個貼圖陣列（陰影 sampler2DArrayShadow 第 4 號、遮罩 R8 陣列第 1 號，一型一層）。換完之後一對一縮圖五種畫質逐位元組同 —— 陣列取樣跟 2D 取樣同值。
- 畫陰影圖之前要先把第 4 號解綁（綁著 ⇒ 讀寫同一張）。
- GPU 的格只給「視野裡最近的 N 艘」（gpuSlots），上一格在裡面的打九折，不然兩艘差不多遠時每一格換、陰影閃；排不進去的畫進一張全局陰影圖。
- 補丁又把 // 註解插進一行中間（60_render.js 瞄準圈那一行，第三次）⇒ 後半行被吃 ⇒ node --check 當場抓到。改完一律 node --check js/*.js。
- sed -i 會把 CRLF 檔變 LF（52_fleet.js、05_hull.js 又中一次）⇒ 改完 file js/*.js | grep -v CRLF。
