---
id: persona-registry-retirement
title: persona registry 退場（AwakenInit/personas → profile 分家＋接縫＋快照）
status: active
created_at: 2026-08-19
related_topics: []
key_docs: []
---

把 persona 檔的 23 欄拆家：路由留專案層、身分進 letters/<p>/profile/、快取欄刪除。Phase 0 先蓋讀寫接縫（persona_profile 兩端）＋解析單端化（A+B：Cmd 主路徑＋快照備援）＋寫入審計（actor/reason 必填）。連動：presence 收斂、過期機制移除、now_status。
