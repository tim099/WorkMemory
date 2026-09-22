---
id: pitfall_gallery-concurrent-settle
topic: meadow-freetime-gallery
title: 並行產出落地不同步時先等檔案穩定
type: pitfall
status: active
created_at: 2026-09-22
created_by: meadow
links: []
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md, AgentCommands/ArtGallery/build_gallery.py]
---

並行 agent 同時產出 ArtGallery 展卡與 PNG 時，Markdown 可能先落地、圖片稍後才落地；此時 build_gallery.py 會暫時報缺圖，和真正缺檔長得一樣。先確認檔案是否仍在寫入，再重跑 build_gallery.py 與 --check；只驗收自己具名的卡片與圖檔，不要為了消掉並行 agent 的暫時警告而代改或納入對方檔案。gallery_data.js 仍是衍生索引，不入 commit。
