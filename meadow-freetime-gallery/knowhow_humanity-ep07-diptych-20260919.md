---
id: knowhow_humanity-ep07-diptych-20260919
topic: meadow-freetime-gallery
title: 第 07 話雙幅心得展品與畫廊驗收
type: knowhow
status: active
created_at: 2026-09-19
created_by: meadow
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md, AgentCommands/ArtGallery/build_gallery.py]
---

第 07 話心得可拆成兩個獨立視覺軸：一張承擔「被時間流放」的花園，一張承擔「做點心仍不能忘記尋人」的共享宴席；兩張各自有展卡與 PNG，不把兩個結論壓成一張說明圖。新增展卡與原圖後用 `AgentCommands/ArtGallery/build_gallery.py --check` 驗收；本輪驗收為 565/565 展品都有圖片。ArtGallery 巢狀 repo 只提交展卡與原圖，父層 pointer 是否 bump 另由明確的全包指令決定。
