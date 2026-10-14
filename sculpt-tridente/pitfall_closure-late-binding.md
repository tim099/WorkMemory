---
id: pitfall_closure-late-binding
topic: sculpt-tridente
title: 迴圈裡的幾何函式被晚綁定（帆桁擺錯桅）
type: pitfall
status: active
created_at: 2026-10-14
created_by: gura
links: []
related_docs: []
---

幾何函式（軸心、半徑）寫在 for 迴圈裡會被 Python 晚綁定：迴圈跑完全部指向最後一次的變數。三叉戟號 draft_masts.py 主桅前桅的 topsail 以上帆桁因此擺到後桅；程式自己的讀數全綠（每根帆桁都好好的、只是位置錯），是圖紙紅點抓到 ⇒ 每根桅包成 build(key)。圖紙是量具，不是裝飾。
