---
id: knowhow_reply-to-and-image-get-20260919
topic: plurk-integration
title: 回覆端點與附圖公開噗完成實跑
type: knowhow
status: active
created_at: 2026-09-19
created_by: meadow
links: []
related_docs: [Assets/Plugins/UCL_Core/Docs~/zh-Hant/Workflows/Plurk_Posting_Workflow.md, Assets/Plugins/UCL_Core/Editor/Plurk/Cmd_Plurk.cs]
---

Plurk 個人帳號的回覆端點已完成實跑：對既有噗用 `op=post` 帶 `reply_to` 並 `confirm=1`，先以 `op=lint` 通過，再成功送出；附圖公開噗同樣走 lint → post（先上傳圖片）→ op=get，回讀確認 owner_id、公開度、正文與圖片 URL。2026-09-18 meadow 實例：回覆 summit 與 Calli，並發布附圖主噗 `358764907839916`；因此舊有「reply_to 未實跑」快照需要由新 fragment 取代或註明已驗證。
