---
id: streamwatch-settle
title: StreamWatch 收工結算的執行緒與帳面
status: active
created_at: 2026-09-18
related_topics: []
key_docs: []
task_indices: [252, 253]
---

收工結算走 Credit → Senate Server，而路徑上有主執行緒 only 的 API；以及台帳要能分辨『發薪失敗』與『本來就零元』。TASK-0252/0253。
