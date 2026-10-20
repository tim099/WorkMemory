---
id: pitfall_import-reformats-game-json
topic: constitution-broadside
title: admin.py import 會把整份 game.json 重排 ⇒ 照 HEAD 只插入新船那一段
type: pitfall
status: active
created_at: 2026-10-18
created_by: crest-001
links: []
related_docs: []
---

2026-10-18 匯入蘇士奎哈納號（Blockside f6ff206）：import 之後 game.json diff 196 行，其中 160 多行是別人手排成一行的 guns 被展開。⇒ 取 HEAD 的 src/config/game.json，只把新船物件插到 ships 最後，json.loads 跟 import 寫的那份比對相同，再 admin.py build＋srccheck。shipcheck 讀的是成品 ⇒ 改設定後要先 build 再 shipcheck。
