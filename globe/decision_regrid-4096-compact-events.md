---
id: decision_regrid-4096-compact-events
topic: globe
title: 球面 regrid（N 2048→4096）＋緊湊事件＋初始格單位（2026-10-08）
type: decision
status: active
created_at: 2026-10-08
created_by: unknown
links: []
related_docs: []
---

## 決定（2026-10-08，Tim 拍板）

- 球面 N=2048 → 4096（regrid ×2）。事件只追加；舊事件不遷移（解析端只有 Senate）。
- 事件改緊湊編碼（Runs＝[起點, 長度, 新值, 舊值]）；舊 Cells 三元組照樣讀得懂。
- radius／width／max_cells 預設單位＝初始格（建立時的格），regrid 後自動換算；`unit=cell` 取實際格數。
- 世界地圖輸出寬度上限維持 8192（N=4096 時是隔一格取樣）。

## knowhow

- 等角網格是巢狀的：舊格邊界必落在新格邊界上 ⇒ 舊畫一格變 f×f 格不位移，舊鋸齒也原樣保留。事件裡的 index 屬於「寫下它那一刻的 N」，由 regrid 歷史推（`SCP_GlobeState.NAt`）。
- 舊版程式的保護靠 meta 的 `Mapping` 改名（`equiangular-cube-v2`），舊版 LoadMeta 大聲拒絕；⛔ 不是版本檢查。寫入順序「meta 標記 → 事件」。
- 每次寫入帶 `iExpectN`；進鎖後 N 變了 ⇒ 整筆拒絕。快取依 N 分夾（`tiles`／`tiles_<N>`）。
- 重畫自己的舊作（refine）：從事件算「每格最後寫入者是不是我的施工區」，只清那些；新陸地只畫在空格；驗收＝全球逐格 diff，範圍外的變動拿事件表對帳（併發時等於別人的事件）。海岸資料用 `build/globe-natural-earth-50m-land.geojson`＋110m 國界當遮罩（shapely 可用）。

## 坑

- Windows 上 `File.Replace` 目標被別人剛好在讀會丟 `Unable to remove the file to be replaced`（regrid 的 meta 寫入、施工區勾項各撞一次）。`SCP_GlobeStore.Replace` 已加重試（≤8 次、約 0.9 秒）；**施工區自己的存檔函式（SCP_GlobeZones.Save）沒有**，撞到就重跑一次。
- `meta.json.tmp` 殘檔：寫 meta 失敗時會留在 Globe 資料夾（進版控的 repo 裡），要手動刪。
- 色號是 RGB332（雕刻）：手算色號兩次不是想要的色，一定要回讀 view；box 不覆蓋已有格，換色先 carve 再 box。
- `senate selftest --only` 跑過一次且通過的新測試會被框架自動關閉（`--enable` 才常駐）。

## 沒驗到的

- SCP_Core 的改動只在 Senate 那份用 `dotnet build` 驗過；Unity 端（LangVersion 9、nullable 關）這台機器沒有 Editor，沒編。
