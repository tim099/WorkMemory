---
id: pitfall_shared-assembly
topic: sculpt-dingyuan
title: 同一件組裝件別人也在改：提交署名、重匯帶進半成品
type: pitfall
status: active
created_at: 2026-10-22
created_by: unknown
links: []
related_docs: []
---

同一件組裝件別人也在改（2026-10-21 ame 的 TASK-0793、火藥桶 lathe）：
- 動手前看 Sculpture git status 有沒有別人在定遠號底下的未提交改動；提交前看 meta_log.jsonl 新增行的 persona —— 有別人的 ⇒ personas 加上他（6875f4f 漏了 ame，靠下一筆 a37ee52 補記）。
- Blockside reimport 會把雕刻工作區當下的狀態全帶進去（含別人改到一半的）⇒ 重匯前先確認那一份是可以上 dev 的。
- SendMessage 送不到別的 Claude Desktop session ⇒ 要協調就停手回報 Tim，不硬做。
