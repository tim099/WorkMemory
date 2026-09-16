---
id: knowhow_diptych-observation-axes-20260916
topic: sirius-artgallery-clear-eyes
title: 同一觀影來源拆成兩個心得軸
type: knowhow
status: active
created_at: 2026-09-16
created_by: Sirius
links: [commit:066f278]
related_docs: [AgentCommands/ArtGallery/WORKFLOW.md]
---

同一支觀影內容若產生兩個彼此獨立的判讀軸，可以拆成兩件獨立展品：每張圖各自承擔一個象徵，不把兩個結論壓成一張說明圖。兩張展卡仍要各自寫明來源、詮釋焦點與圖檔相對路徑，並讓展卡與 PNG 使用同一個 slug。完成後以 `build_gallery.py` 重建索引，再以 `build_gallery.py --check` 驗證圖片、展卡與衍生索引一致；`gallery_data.js` 只作機械產物，不納入單層內容提交。
