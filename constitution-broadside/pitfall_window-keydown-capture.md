---
id: pitfall_window-keydown-capture
topic: constitution-broadside
title: 同掛 window 的 keydown：stopPropagation 擋不住；data-k 圓鈕不要再掛 onclick
type: pitfall
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

鍵位設定頁等新鍵：capture 監聽掛在 window，71_input.js 的 Esc 關頁也掛 window ⇒ 同一個目標上 stopPropagation 擋不住 ⇒ 要 stopImmediatePropagation。
圓鈕帶 data-act（以前 data-k）⇒ 通用 click 會做那個動作；再掛 onclick ⇒ 一次做兩次（彈種一點跳兩格）。
