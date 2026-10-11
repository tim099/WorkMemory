---
id: knowhow_sails-tether-and-render-2026-10-11
topic: sculpture-senate
title: 帆：牽繩長度、比例、另一趟渲染、讀數基準（TASK-0503）
type: knowhow
status: active
created_at: 2026-10-12
created_by: kotoko
links: []
related_docs: [Docs/Workflows/Sculpture.md]
---

帆（TASK-0503，2026-10-11 kotoko）—— 接手要知道的幾格；用法在 Sculpture.md §9.4.7，這裡只記「為什麼」與會咬人的地方。

- **牽繩的長度是用設計位置定的**：L ＝ slack × 設計時兩端距離。slack 下限若是 1，繩永遠比帆角到繩樁遠 ⇒ 拉不動，只在帆被吹離時才繃。所以綁帆角的繩放寬到 0.3（收帆腳索），其他繩照舊 1..10。判斷繃住：讀數的「長 ＝ 直線」；長 > 直線 ⇒ 還在垂。
- **鬆的繩不出力**是約束的定義（只在超過 L 時拉回），不是 bug。憲法號的 course 往上抬，就是 sheet 繃、tack 鬆（slack 1、垂 21–26 格）。
- **比例**：重力與風照**組裝件**的 meters_per_voxel 換算。後來另開的組裝件沒記比例 ⇒ 預設 1/32，力會差 3.2 倍（basecamp 第一輪帆鼓成降落傘）。
- **帆是另一趟渲染**：帆＋綁帆角的繩不併進 voxel 網格（併進去 ⇒ 觀測頁每次動都重建整艘船）。只換帆形狀 ⇒ voxel 網格沿用（GPU 淨室驗過）。
- **觀測頁持續模擬**：每 0.1 秒更新、一次最多約 8 ms（Tim「不用頻繁模擬」）；算不完就慢動作，⛔ 不改 dt（改 dt 就不再是 CLI 那一刻的同一條軌跡）。
- **讀數基準**（Release）：24 面×625 布點每步 6.8 ms；憲法號 15 面 9.4k 布點每步約 5.4 ms。
- **斜的繩比水平懸鏈線公式垂得多**：d ≈ h·√(3(slack−1)/8) 只對水平拉的繩準；斜拉 73 格、slack 1.01 實測垂 12.6、公式給 4.5。長繩一律拉直（slack 1）。
- 沒做：貼圖（要 UV＋shader 改動）、收帆／縮帆、碰撞。
