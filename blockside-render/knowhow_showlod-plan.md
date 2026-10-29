---
id: knowhow_showlod-plan
topic: blockside-render
title: 表演船遠近分級（TASK-1104）讀完程式的接法：中 P.vox×4、遠照城鎮貼圖程式、交接帶拉出合批、照 shipcheck 那條路烘
type: knowhow
status: active
created_at: 2026-10-30
created_by: unknown
links: []
related_docs: []
---

**表演船遠近分級（TASK-1104）讀完程式的接法 —— 還沒動工**（erina 2026-10-30 凌晨）

畫船的現況（Blockside dev db401d1）：
- 船殼：60_render.js drawHulls → 60_rbatch.js rbatHull（照船型的合併緩衝一段一次畫，P.vox，uModel ＝ sh.M）。格子網格的格式跟城鎮模型同一種：每個頂點 uvec4（Uint16 x、y、z、面｜色碼 << 3），一個四邊形 4 點、6 個索引。
- P.vox ＝ VK.VOXEL_VS ＋ voxFS()：vL ＝ 格座標（每格明暗雜湊、格邊變暗都照它），點光源比 vL 跟 uVG（格座標）、剖面用 uCutL·vL。
- 模型、裝飾、平滑面、裝甲的平滑面：rbatPrep → rbatShip 一格整理一張表；反射、主畫面 rbatColour 用 P.triI 跨船合批（不管先後），陰影 rbatDepth 收桶。
- 會動的模型：rbatModelL 走 modelLocal 的那幾種（m.swM／m.sw／spinB.mt／rot＋piv（帆桁）／m.hd（砲塔））、m.vox 螺旋槳、錨（ancD）、砲門縮進（vsl）。

接法：
- 中：降採樣的格子用 **P.vox 原樣**畫，uModel ＝ sh.M × 縮放 4（vL 變成 0.4 m 一格）⇒ 光照、霧、陰影同一套；點光源 uVG 的位置與半徑也除以 4；uCutL 給縮放 4。rbatShip 裡把不會動的模型、裝飾、平滑面跳過（陰影、反射、主畫面三趟都照同一張表 ⇒ 一處改）；帆、繩、會動的模型照舊。
- 遠：一支貼圖程式（照 63_towns.js TOWN_BB_VS／FS，同一套固定光 × 天色）；遠那一級在 rbatShip 整艘跳過、drawSails／繩／船員跳過；不投影、不給海面遮罩（gpuSlots）。
- 近↔中交接帶的抖動：rbatColour 是跨船合批 ⇒ 交接帶裡的船要從批裡拉出來（同 viewVis 鏡頭外那樣 rbatGRes 保持組的先後），另外用 P.tri／P.vox 的分身程式畫（多一個 uniform 距離＋抖動 discard）；陰影不抖（照主要那一級）。
- 烘：照 admin.py shipcheck 那條路 —— 無頭 Chrome（SwiftShader）開成品 `?shiplod=<id>`、頁面把結果寫進 `<pre id=...>`、`--dump-dom` 抓（75_shipcheck.js done() 同一種）。貼圖大到 dump-dom 不好帶 ⇒ 可以改用 render_lab cdp.py 那種 CDP（src/tests/ambient_lab.py 的理由）。
- 存：ships/<el>.lod.js（script 標籤載，同 flagloop.js；ships_sync 要放過 *.lod.js）；指紋對 config/vxk_lock.json 那一艘的 c:sha1（成品 blockside-src 的 files 裡有 lock c:前 12 碼可讀）＋格式版本 ⇒ 對不上就那一型不分級。
- 城鎮降採樣（Blocksail tools/townmodels.py downsample）：4×4×4、64 格裡滿 12 格（LO_FILL）、取最多的那一色；貼圖是 CPU 點描（impostors），船的帆只有遊戲生得出來 ⇒ 船的貼圖要在頁面裡用 GL 拍。

會咬人：
- render_lab：harness.py 在 python -I 下印中文會 UnicodeEncodeError（量完才崩）⇒ `python -I -X utf8`。
- 量測頁的 JS 經 curl 參數傳，中文會變 ? ⇒ 寫 \u 跳脫。
