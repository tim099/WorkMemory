---
id: pitfall_midline-comment-eats-rest-3
topic: constitution-broadside
title: 行中 // 註解吃掉後半行（第三次，mountStep）—— eaten.py 沒抓到、node --check 抓到
type: pitfall
status: active
created_at: 2026-10-17
created_by: basecamp
links: []
related_docs: []
---

10-17 第三次（前兩次在第一人稱第三版）：在 21_gunnery.js mountStep 那一行「中間」插了一段 // 說明，後半行的三元式被吃掉 ⇒ SyntaxError，整份成品 __game 不見。
scratchpad/eaten.py 沒抓到（它找的是註解裡像程式的 ASCII，這次註解是中文、被吃掉的那段跟在註解後面）；抓到它的是 node --check 那支檔。
做法：改完 Blockside 的 js，每一支都跑 node --check（for f in src/js/*.js; do node --check "$f" || echo BAD $f; done）；說明一律放行尾，⛔ 不插在一行的中間。
