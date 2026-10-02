---
id: knowhow_discord-mention-rewrite
topic: senate-backend
title: Discord 轉發 @ 通知：常駐送出才換 <@id>，手動補發不通知
type: knowhow
status: active
created_at: 2026-10-02
created_by: summit
links: []
related_docs: [SCP_Core/Docs~/Discord_Relay.md]
---

Discord 轉發的 @ 通知（TASK-0380，SCP_Core 6f604a3）：
- 只在**常駐送出**（SCP_DiscordOutbound.SendNew → Backfill(iPing:true)）把內文登記過的 `@名字` 換成 `<@id>`，並只把那幾個 id 放進 `allowed_mentions.users`；`parse` 永遠空陣列（不解析 @everyone／@here）。
- **手動 op=backfill 不換、不通知**（iPing 預設 false）—— 補發舊訊息不該把人叫起來。新增 Backfill 呼叫端時要想清楚傳不傳 iPing。
- 名字對照（SCP_DiscordMentions.LoadMap）：① notify_config.json `tavern_mirror.discord_user_mentions` 優先 ② 白名單顯示名稱＋aliases 補缺。名字後面要接空白或標點才斷得開（`@熊汁我是` 不會通知）。
- 🩸 這是 TASK-0316 Unity→Senate 搬家漏掉的一步（舊 UCL_DiscordIdentityResolver.RewriteMentions），漏了一整週沒人發現 —— 因為失效樣子是「訊息照常出現在 Discord，只是名字是灰的」。
- ⚠ 一個分類綁多條 webhook ⇒ 被 @ 的人會收到多次通知（沿用設計，未改）。
