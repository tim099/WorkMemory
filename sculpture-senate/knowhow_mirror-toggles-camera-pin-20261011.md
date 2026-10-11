---
id: knowhow_mirror-toggles-camera-pin-20261011
topic: sculpture-senate
title: 鏡像／觀測頁關聯開關／GPU 預覽鏡頭釘住（TASK-0497/0499/0500/0501）
type: knowhow
status: active
created_at: 2026-10-12
created_by: unknown
links: []
related_docs: []
---

2026-10-11 雕刻四張（apex-one）：0497 鏡像、0499／0500 觀測頁關聯開關、0501 GPU 預覽鏡頭釘住。
- 鏡像：關聯與 assemble 共用 `SCP_SculptLinks.Transform`（先在零件自己的框裡翻 x→W−1−x，再 turn，再 at）。links.json 有任何一條用鏡像就寫 **schema 2**（SCP_SculptLink 沒有收容所，舊 exe 會默默丟 mirror ⇒ 升號讓它 exit 2）。CLI 的 `mirror` 鍵與 stats 的「量鏡像差」同名、兩種寫法互不通用。
- 觀測頁開關：開關 → links=（畫的全部點名）＋hide_links=（關掉而上一層有畫的）。⚠ 只送 links= 會把點名那條底下沒點名、但 enabled 的子關聯畫回來。畫不畫只問 DrawnFlags（Unity activeInHierarchy）；折疊只管列不列、預設折起來。三角形用 `SCP_Ui.FoldArrow`（ImGui ArrowButton，跟勾選框同大）。
- 透視 Lag 成因：SolveCamera 自動框住每幀把外露 voxel（鏡廳 80 萬）掃約 14 遍。修法：渲染器把實際用的鏡頭經 GPU 狀態 Result 交回（SCP_SculptResolvedCamera），頁面釘住 target＋distance／zoom 送回 ⇒ 不再框（鏡廳 387→65 ms）。另一個驗過但沒做的加速：框景點只留「三軸線端點」（凸包頂點的超集），鏡頭矩陣逐位元相同、點數剩 0.2–4.5%。
- 驗「釘住」要兩格：同一刻逐位元相同（沒做錯）＋換視角後中心距離不變（真的釘住）；只驗前者，「每次重算」跟「釘住」同形（突變對照實證）。
- 帆的風：作品要自己設 meters_per_voxel（沒設 ⇒ 1 m＝32 格，風壓換算錯三倍多）與 `work sub=wind`；鏡像進來的帆在觀測作品的世界座標裡模擬、重力 −z ⇒ 倒影的帆不是逐格鏡像。
