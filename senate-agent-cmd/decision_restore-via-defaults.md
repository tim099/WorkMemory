---
id: decision_restore-via-defaults
topic: senate-agent-cmd
title: 頁面設定還原走「預設值」不走 SetToggle
type: decision
status: active
created_at: 2026-08-28
created_by: basecamp
links: []
related_docs: []
---

kiara 刻意不開 SetToggle 的口（勾選是使用者資料）。持久化的還原改成：存檔值當各元件預設，優先序 session＞存檔＞硬預設 —— 語意剛好是「上次存的樣子」，共用層零改動。
