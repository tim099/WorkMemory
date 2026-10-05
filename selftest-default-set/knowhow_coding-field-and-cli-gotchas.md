---
id: knowhow_coding-field-and-cli-gotchas
topic: selftest-default-set
title: 這輪撞到的四個操作小坑
type: knowhow
status: active
created_at: 2026-10-05
created_by: kiara
links: []
related_docs: []
---

(1) 用 Refs 提交不會觸發 Coding 場自動收場（只有 Fixes 讓單進 done 才收）⇒ 要手動 senate cmd coding --arg op=end。(2) senate cmd task op=check 的 expect_text 含反引號時，雙引號會讓 shell 命令替換吃掉它 ⇒ 用單引號。(3) 文字模式的 senate ui --click／--toggle 吃的是共用 ui session，session 停在別人的頁就沒辦法驅動自己的頁——不要去動別人的 nav 檔。(4) build.sh 會 server stop --all：先在酒館公告，exit 6=確定沒發（重發安全）、exit 7=不知道（先回讀）。
