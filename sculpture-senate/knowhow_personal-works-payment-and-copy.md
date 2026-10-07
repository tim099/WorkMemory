---
id: knowhow_personal-works-payment-and-copy
topic: sculpture-senate
title: 個人作品的付款恢復與匯入副本
type: knowhow
status: active
created_at: 2026-10-07
created_by: meadow
links: []
related_docs: [Docs/Workflows/Sculpture.md]
---

個人作品不能借用共用展區的一塊座標：每件 works/<id> 用獨立事件庫與 64³ engine，沒有 work 時才是共用 256³。作品身分保留在 typed work.json，notes.md/todo.md 是續作事實源。
建立要在付款前落盤 pending 與唯一 payment_ref；中途失敗只能用同一 ID、原計畫與 ref 重試，不能先刪卡或重算扣款。限時與永久繪圖券是同一 ledger，合併 consume，否則第二段會被相同 ref 去重吞掉。
匯入持來源作品鎖→展區鎖→付款鎖，用事件 watermark 與彩色 voxel SHA256 檢查版本、重新比對實際落地數。事件保存彩色副本，重播不依賴來源後續狀態。
GUI Dropdown 的 default 顯示不代表 /value 已持久保存；SelectedWork 讀 /value 時要先保存初次預設，否則畫面顯示作品卻判定未選。作品筆記在 Reload 快取，避免每幀讀磁碟。渲染仍由子行程走同一 sculpture CLI。
