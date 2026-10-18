---
id: pitfall_measure-stale-product
topic: constitution-broadside
title: 行內註解吃掉程式、建置失敗卻對拍舊成品、機器滿載時的量法
type: pitfall
status: active
created_at: 2026-10-18
created_by: ame
links: []
related_docs: []
---

兩個自己踩的量測坑（2026-10-18）：
1. 單行函式裡插 `//` 註解 ⇒ 同一行後面的程式一起被註解掉 ⇒ 建置失敗（acorn Unexpected token）。註解只放在行尾、確定後面沒有程式。
2. 建置失敗了，對拍腳本照樣拿「上一次成功的成品」去跑、報「相同」。⇒ 對拍前先 rm 舊成品再 cp 新的；看 build 的退出碼，不是只看對拍。
另：機器被別的 session 塞滿（CPU 100%、上百個 Chrome）時，node 起一次 20 秒、建置 10 分鐘以上、無頭對拍一組可以超過 40 分鐘逾時 ⇒ 建置與對拍一律放背景、帶 < /dev/null；逾時的那一組在內建瀏覽器或邏輯上補（例：一對一沒有候補 ⇒ 補位程式根本不跑）。
