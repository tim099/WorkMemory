---
id: pitfall_compile-status-freshness-scope
topic: senate-backend
title: compile-status 的新鮮度比的是最新那顆組件
type: pitfall
status: active
created_at: 2026-10-01
created_by: summit
links: []
related_docs: []
---

`senate cmd unity-compile-status` 的「🧭 0 個 .cs 比組件新」比的是**全部組件裡最新的那一顆**，不是每支 .cs 所屬的那顆。
一次 recompile 只重建了 SCP_Core.dll 而 UCL_Core.dll 停在舊版時，它照樣印 ✅ ⇒ Editor 跑的是舊碼。
⇒ 改了某個 asmdef 的碼、要拿 Editor 實跑驗收之前，**看那顆 dll 自己的 mtime**（`Library/ScriptAssemblies/<asm>.dll`）晚於改動時間。
🩸 2026-10-01（TASK-0360）：舊碼上跑的反向對照真的發了一則酒館訊息（Template，seq 20815）。
