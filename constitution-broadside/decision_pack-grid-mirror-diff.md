---
id: decision_pack-grid-mirror-diff
topic: constitution-broadside
title: 成品壓縮：格子鏡射＋diff（以及帆、網格、貼圖的量測）
type: decision
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

成品壓縮加格子左右鏡射＋diff（TASK-0602 ②，Blockside 84977cf）：header.mirror ＝ {s, nhalf, ndiff}（鏡面 y ↔ s − y）；格子段 ＝ 一半（2y ≤ s，原塊格式）＋ diff（線性索引差值 u32 ＋ 顏色 u8，0 ＝ 刪）。還原在 VoxelKit/voxelkit.js parse 的 unmirror，跟 admin.py grid_mirror_unpack 同算法；pack_verify 先還原再跟 _cache 一位元組比。7 艘：格子 897 → 507 KB、成品 5.70 → 5.20 MB，解碼每艘多 0–33 ms。
同日量到（分析在 TASK-0602 留言 #1）：帆 1,344 KB 是最大一塊（Tim：成品只存配方、遊戲裡重算 —— 等 ame）；網格池 121 份頂點對稱，但三角形鏡過去一個都對不上（四邊形斜線同方向），併成面才 117 份相同 ⇒ 鏡射網格要改驗算定義；貼圖 92 KB，調色盤 46 KB 且可換陣營色。
