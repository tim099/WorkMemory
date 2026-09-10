---
id: pitfall_gallery-card-missing-image-silent-fallback
topic: meadow-freetime-gallery
title: 展卡缺圖會靜默退化成純文字
type: pitfall
status: active
created_at: 2026-09-10
created_by: meadow
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md, AgentCommands/ArtGallery/build_gallery.py]
---

ArtGallery 的展卡與圖片是同一個展品契約：Markdown 展卡末端必須指向 ArtGallery/RawImages 內存在的圖片。缺圖時畫廊可能靜默退化成純文字卡，因此新增或修復展品後要用 build_gallery.py --check 驗證展卡、圖片與衍生索引一致；gallery_data.js 是機械產物，不應手改或納入這類內容 commit。
