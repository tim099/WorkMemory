---
id: pitfall_chunk-new-segment
topic: blocksail-world-globe
title: 海圖塊加新段：段 4 解碼讀過頭、包舊 chunks.js 的坑、快取指紋
type: pitfall
status: active
created_at: 2026-11-06
created_by: basecamp
links: []
related_docs: []
---

海圖塊加新段的三個坑（TASK-1234，2026-11-06）：
1. Blockside bedDecode 段 4（河的水流）以前一直讀到整塊最後一點 ⇒ 接在後面的段被當成水流吃掉、新段讀不到（舊版只掃過乾的點、不寫錯值）。e8175d5 改成有水的點讀完就停。新段一律接在段 4 之後（放前面 ⇒ 舊解碼器遇到不認得的記號就停，段 4 也讀不到）。
2. 驗「沒有新段的塊逐位元組相同」：地球儀頁裡把主線的 world/chunks.js 包成另一份 BSW（檔尾是 `})(typeof window !== 'undefined' ? window : self);`，要換成自己的參數 —— 包錯會直接蓋掉頁面的 BSW，比出來是舊對舊），同一個狀態兩份各產一次比位元組；舊解碼器從共用 battle.html 照括號配對抽出 bedDecode。
3. world/chunk_store.js 的快取指紋算 chunks.js 的內容（CHUNK_CODE）⇒ 改一個字，玩家的 IndexedDB 海圖塊全部重算一次；chunk_bake.bin 不在 git，打包時重烘。
