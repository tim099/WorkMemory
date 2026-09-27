# 工作記憶索引 — tavern-senate-migration

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_d10-explicit-writer-switch** — D10：酒館寫入走顯式開關，⛔ 無自動降級（Tim 2026-09-20 拍）  ↔ senate-backend/decision_server-identity-serverid
- **decision_tavern-preprocess-all-in-scp-core** — 發文前處理四段各住哪（schema／CLI／creative 寫入端寄／alter 延後發文匣）

## pitfall
- **pitfall_deferred-outbox-and-glossary-prefix** — 延後發文匣先認領再送／沒有 post_seq／詞典顯示前綴要逐字 docs/Glossary
- **pitfall_write-seam-is-appendmessage** — 切換點是 AppendMessage 那一個函式 —— 外部呼叫端 21 處，只有 3 處在 Cmd_Tavern
