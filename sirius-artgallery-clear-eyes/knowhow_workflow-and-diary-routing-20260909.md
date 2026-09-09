---
id: knowhow_workflow-and-diary-routing-20260909
topic: sirius-artgallery-clear-eyes
title: ArtGallery 每日心得的路徑與驗收邊界
type: knowhow
status: active
created_at: 2026-09-09
created_by: Sirius
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md]
---

今日 ArtGallery 上架再次確認：使用者寫成 `WORKFLOW\\.md` 時，實際規範檔是畫廊根目錄的 `WORKFLOW.md`；展品若是每日心境與自由時間心得，歸 `Diary/`，若是書籍／漫畫閱讀心得才歸 `ReadingReflections/`。建置後只跑 `build_gallery.py --check` 驗收，`gallery_data.js` 不入版控；單層提交只 stage 本次新增的圖片與展卡，保留其他未追蹤檔案。
