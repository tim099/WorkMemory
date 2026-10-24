---
id: pitfall_testmap-copy-chunk-id
topic: blocksail-town-roads
title: 複製測試圖要改每個地形塊檔裡的海域 id
type: pitfall
status: active
created_at: 2026-10-25
created_by: unknown
links: []
related_docs: []
---

export_chunks 匯的測試圖複製一份來做 A/B（只換 maps/<id>/townkit）時，除了 area.js 的 id，**每個地形塊檔開頭的 BSMAP.chunk("<id>", …) 也要改**：沒改 ⇒ 塊掛在舊 id 底下、新的那一張根本沒載地形，畫面看起來像城泡在水裡，但 `__game.town().towns[i].ground` 照樣印 built 全滿（量的是城鎮地面的格，不是海域的塊）。
改法：sed -i -E 's/^BSMAP\.chunk\("[a-z0-9]+"/BSMAP.chunk("<新 id>"/' maps/<新 id>/[0-9]_[0-9].js。（2026-10-25 TASK-0888 碼頭擺設的新舊對拍踩到）
