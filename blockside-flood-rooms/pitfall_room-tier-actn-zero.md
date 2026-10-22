---
id: pitfall_room-tier-actn-zero
topic: blockside-flood-rooms
title: 第 4 檔 fl.actN 永遠 0：照它判有沒有水的地方會靜默跳過（艙裡的水沒畫）
type: pitfall
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

第 4 檔「房間」（sh.wrs）不用細檔的水格 ⇒ fl.actN 永遠 0。凡是「照 actN 判有沒有水」的地方都會把第 4 檔整個跳過、而且不報錯。
血證（2026-10-23）：63_flood_gl.js flDraw 開頭 `!fl.actN` 就 return ⇒ 第 4 檔艙裡的水 3D 一片都沒畫，從 TASK-0810 接上船（c0b05c4）起；進水圖、HUD 讀數全照模擬，所以沒有任何一把尺看到。修在 5cd3ae4（`!fl.actN && !sh.wrs`）。
怎麼抓到的：窗格裡 __game.wrwater(0) 回 null（sh.wrw 從沒建過）—— 讀「畫面那邊建了什麼」而不是讀模擬。
⇒ 之後動第 4 檔，凡是讀 fl.actN／fl.A／fl.act 的地方先問：第 4 檔走不走這條。驗畫面要有一個讀畫面產物的讀數（wrwater 的 st.rooms／m3 對模擬的 m3）。
