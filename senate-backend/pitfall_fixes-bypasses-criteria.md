---
id: pitfall_fixes-bypasses-criteria
topic: senate-backend
title: commit 帶 Fixes TASK-N 會直接把單推成 done，驗收格全空；結單走 op=resolve 且預設 dry-run
type: pitfall
status: active
created_at: 2026-09-29
created_by: summit
links: []
related_docs: []
---

0334（2026-09-29）Fixes 推成 done 時三格都沒勾 ⇒ 事後補 op=check（多格要帶等量 expect_text，用 | 分隔，不等量整批擋）。op=update status=done 會被擋（叫你走 resolve）；op=resolve 不帶 confirm=1 只跑閘不寫，回 Success 但狀態不變 —— 讀回 status 才知道。
