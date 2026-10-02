# 工作記憶索引 — tavern-senate-migration

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## decision
- **decision_msgindex-writer-maintained** — 訊息索引由寫入端維護，讀取端只讀
- **decision_tavern-preprocess-all-in-scp-core** — 發文前處理四段各住哪（schema／CLI／creative 寫入端寄／alter 延後發文匣）
- **decision_writer-is-server-only** — 酒館寫入端只剩 Server，tavern.writer 開關已拔（拔的順序會咬人）
- **decision_d10-explicit-writer-switch** — D10：酒館寫入走顯式開關，⛔ 無自動降級（Tim 2026-09-20 拍） ~~[superseded]~~  ↔ senate-backend/decision_server-identity-serverid, tavern-senate-migration/decision_writer-is-server-only

## pitfall
- **pitfall_deferred-outbox-and-glossary-prefix** — 延後發文匣先認領再送／沒有 post_seq／詞典顯示前綴要逐字 docs/Glossary
- **pitfall_tavern-queue-0372-traps** — 0372 排隊：淨室走不到 not_running／只停 Server 不夠／mv 還原不重編／新態別落進預設分支
- **pitfall_write-seam-is-appendmessage** — 切換點是 AppendMessage 那一個函式 —— 外部呼叫端 21 處，只有 3 處在 Cmd_Tavern
