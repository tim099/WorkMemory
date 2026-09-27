---
id: decision_tavern-preprocess-all-in-scp-core
topic: tavern-senate-migration
title: 發文前處理四段各住哪（schema／CLI／creative 寫入端寄／alter 延後發文匣）
type: decision
status: active
created_at: 2026-09-27
created_by: basecamp
links: []
related_docs: []
---

**發文前處理全部在 SCP_Core（TASK-0311／0312，2026-09-27）—— 各段住哪、為什麼住那裡。**

- meta schema（commit／task-assign／task-ack）⇒ `SCP_TavernMetaSchema`，**寫入前**擋；不合＝確定沒發（兩個入口同一句）。
- 酒保 CLI 前綴 ⇒ `SCP_TavernCli`，在 compose 裡判（打 `cli-cmd`、不附詞典）。設定不存在／讀壞＝enabled＋cmd（照 Editor，fail-closed）。
- creative 留念信 ⇒ `SCP_TavernCreativeArchive`＋`SCP_RegisteredMail`，**寫入端寄**（Senate `Cmd_TavernWrite.AppendCreativeArchive`／Editor `AppendMessage` 本地寫那條），同 @mention／發薪。理由：信要 seq，而延後發文排程時還沒有 seq；放寫入端 ⇒ 每條路恰好一封。⛔ 別搬回 op=post（writer=server 時會寄兩封）。
- alter 配對延遲 ⇒ `SCP_TavernAlterPacing` 只判不等。Editor 在 handler 裡 await；Senate 放進 `<酒館 Server 根>/deferred/`（`SenateTavernDeferred`），Server 心跳 `FlushDue` 到點投回自己的 tavern lane。⛔ 不在 CLI sleep（Bash 前景 120s）、⛔ 不在 tavern lane sleep（串行卡全體）。
- 發文閘（commit 公告／小歇）**刻意不做 alter 延遲**：那是公告不是對話（同 Editor op=share 的 bypass）；唯一交回 Editor 的理由是「找不到資料根對應的專案根」。
