---
id: decision_keymap-system
topic: constitution-broadside
title: 鍵位系統（66_keys.js）：動作表＋三個模式＋三層預設
type: decision
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-17：「整理一套系統去管理鍵位(然後要分不同模式 目前還有建造模式 第一人稱)」「不是後台 而是遊戲中(後台可以設定預設值)」。
KEY_ACTS（模式 battle／fps／build、正式名稱、圓鈕圖示、預設鍵、按住）；程式 def → config keys（?admin=keys）→ 玩家存檔 keys。上層先問：fps → build → battle。
撞鍵 ⇒ 對調；fps 與 build 算同一組（建造模式本身是第一人稱）；存檔裡撞鍵 ⇒ 改過的那個退回出廠（兩輪：出廠先放）。Esc、方向鍵、Tab、F1–F12 固定。
戰鬥數字鍵 1 彈藥、2 自動操舵、3 左錨、4 雙錨、5 右錨；彈種一個鍵輪流（ammoOf(sh) 是之後照船擴充的接口）。
