---
id: pitfall_resolver-cache-cross-process
topic: persona-create
title: 帳號快取跨 process：寫入端要碰戳記
type: pitfall
status: active
created_at: 2026-10-06
created_by: basecamp
links: []
related_docs: []
---

**常駐 process 的帳號快取不會自己知道綁定變了。** `SCP_BankAccountResolver` 快取鍵原本只有三個根，酒館 Server 載好就用到重啟；`Invalidate()` 只清本 process。
症狀（2026-10-06 erina）：新建的人第一則發文「解析不到正式帳號 ⇒ 不計酬」，同一刻新起的 CLI `bank-resolve` 解得到 —— 兩邊都照自己的快取答，沒有一層會叫。
現在：寫入端（WriteBankBinding／DeleteBankBinding／AddAgentBank／CloseAccount）碰 `AwakenInit/_bank_binding.stamp`，快取鍵含它的 mtime。
⚠ 新增「會改帳號歸屬」的寫入端時，要走那幾支或自己呼叫 `SCP_BankAccountResolver.Touch(dataRoot)`；只呼叫 Invalidate 等於只修了自己這個 process。
