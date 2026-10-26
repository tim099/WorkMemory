---
id: knowhow_river-navigation
topic: blocksail-architecture
title: 內陸水體：河開得進去要水面磨平、河口改線、要多深三件一起對（TASK-0768）
type: knowhow
status: active
created_at: 2026-10-26
created_by: unknown
links: []
related_docs: []
---

河開得進去要三件事一起對（WORLD_FORMAT §21「航行」）：
1. 水面：water_profile 的水面照生成地形的谷底取「往下游不增」⇒ 一段平接一道陡階；航行段要 tools/water_nav.cjs 磨（每公里 0.3～1 m）。
   生成地形在等高線稀的地方（金角灣頭只有 0、50 m）會把河谷墊成 40 m 台地 ⇒ 加 --rejoin（急流接回原本水面）＋河的 valley／vslope（寬谷），不然頂上一道假瀑布、或挖成峽谷。
2. 河口外：海底資料常常淺（北亞克河口外 0.3～1.4 m、金角灣頭 <2 m）⇒ 照港口的作法加一筆改線（bathy_edits lines，harbour_deep.py），不另外畫區。
3. 船：要多深 ＝ 吃水 ＋ 0.5（WATER_NAV_CLR，河道深是資料，不加粗圖誤差 4）；地球儀的起點、Blockside 避淺都照這個數。

抓 OSM：這台機器 Overpass 的幾個鏡像都回同一句「at capacity」（像是中間被擋）⇒ 用 Nominatim 查 relation id（名字照 OSM 的寫法，Kâğıthane 有長音符號）、api.openstreetmap.org/api/0.6/relation/<id>/full 抓整筆，main_stream 照點的順序（＝水流方向）接成中線；要帶 User-Agent。
