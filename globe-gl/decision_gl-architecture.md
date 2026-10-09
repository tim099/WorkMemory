---
id: decision_gl-architecture
topic: globe-gl
title: 球面／雕刻 GPU 架構與建築方向（Tim 2026-10-09）
type: decision
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-09 拍板：要在地球上放 3D 建築（之後是城建模擬）⇒ 球面走 GL。建築＝雕刻作品匯出的體素模型＋設定檔（建造費用；功能細節未定）；直接以模型放置、⛔ 不轉地表 voxel（要 culling／遠距低模或貼圖）；尺度可切換（參考雕刻觀測頁）；CLI render 畫不畫建築有選項。

架構（已做）：
- 即時預覽一律在**主視窗的 GL context** 畫進 FBO 貼圖交給 ImGui，⛔ 不讀回。頁面只放場景（SCP_GuiGpuViews `gpu:`），宿主登記畫家；文字模式／`SENATE_GLOBE_GPU=off`／`SENATE_SCULPT_GPU=off` ⇒ 退回 CPU／spawn 並印原因。
- 球面：全畫面三角形＋fragment shader 解析求交（取格與 CPU 同式）並寫深度；格子照 256² 分塊上傳（2D 陣列圖集＋R32I 索引貼圖），逐塊比內容只傳變了的。
- CPU 是格子回讀的正本：CLI render／export 不走 GPU（GPU float 在格子邊界偶差一格，對拍上限 0.5%，實測最差 0.148%）。
- 雕刻：同一份 SenateSculptRenderer 加「外部 GL」模式；網格快取鍵＝清單物件＋顆數＋內容雜湊＋AO／合併旗標；MergeFaces 預設關（CLI 逐位元不變）、觀測頁開。
- 建築（0471）要用的：球面已寫深度；精度要在 CPU 用 double 算鏡頭相對的 model matrix（float32 在公尺尺度會抖）。
