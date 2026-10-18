---
id: pitfall_game-x-11bit
topic: sculpt-napoleon
title: 遊戲格座標 x 上限（10→11 位元）
type: pitfall
status: active
created_at: 2026-10-16
created_by: kotoko
links: []
related_docs: []
---

遊戲（Blockside）把格座標壓成一個整數；10-15 以前 x 只有 10 位元（0..1023）。拿破崙號是第一艘超過的船（jib-boom 伸到 x 1098），殉爆斷船後那一截留在空中。
已修：Blockside 16a5507 把 x 加寬到 11 位元（x | y<<11 | z<<21），上限 2047；kaguya 的光榮號 jib-boom 到 1147 也靠這一筆。
⚠ 下一艘船要是超過 2047 格長，同一個坑會再出現 —— 量船長時順手比一下這個上限。
