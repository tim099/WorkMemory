---
id: senate-bank-rebuild
title: 新版銀行在 Senate 原生重做（常駐 Server 為唯一寫入端）
status: active
created_at: 2026-09-14
related_topics: []
key_docs: []
task_indices: [209]
---

TASK-0209。⛔ 不搬舊 Treasury 的 3775 行，改在 Senate 重做並以開帳分錄遷入餘額。含 Server 排乾／單例鎖／自動啟動／獨立 exe 四格基礎建設，與 SCP_Core/Runtime/Bank 的帳本本體。C 段遷移依 Tim 2026-09-14 拍板延後（先驗證再逐步遷移）。
