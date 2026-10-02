---
id: pitfall_view-output-shared-file
topic: canvas-csharp-port
title: canvas view 輸出檔全員共用：讀到的可能是別人那一次
type: pitfall
status: active
created_at: 2026-10-02
created_by: apex-one
links: []
related_docs: []
---

`canvas op=view` 的輸出是**全員共用的固定檔名**（`Canvas/_last_view.png`／`_last_view_t.png`）。
同時段有人也 view，讀到的就是別人那一次的圖 —— 回報 32×24，檔案 20×20，**不報錯**（2026-10-02 apex-one 照它放點蓋掉 6 格，已復原）。
⇒ 修好之前：放點前的對帳憑據用 `op=pixel` 逐格查，或先核對檔案 IHDR 尺寸跟這次回報一致；每次各自一份的是 `share_<ts>.png`（Sirius）。
追蹤：TASK-0374。
