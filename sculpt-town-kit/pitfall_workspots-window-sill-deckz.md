---
id: pitfall_workspots-window-sill-deckz
topic: sculpt-town-kit
title: 建築工作點：水上版量平台、窗台當工作檯 ⇒ --inside、高機具手放
type: pitfall
status: active
created_at: 2026-10-28
created_by: unknown
links: []
related_docs: []
---

建築裡的工人（Blockside townWorkPrep）照模型的 work-* 點放人：沒有點（也沒有附屬的田）⇒ 整棟跳過，帳上有人上工、畫面沒人。2026-10-28 TASK-1070／1075 補了 59 款。

找點 tools/workspots.py 的三個坑（都修了或加了選項）：
- 水上版（meta deckz）：以前只收離模型底 ≤ 0.8 m 的地板 ⇒ 平台被濾掉、底下 z 0 的海面反而留下，建議的點全在海上 ⇒ 現在照 deckz 從平台面量。
- 窗台會被當成工作檯（窗台以下實心、窗洞空著、腰高有東西又不是整柱牆）⇒ 空的大廳（教堂、總督府、市政廳）點全跑到屋外屋簷下、前院圍籬邊對著牆 ⇒ 服務、公職建築一律加 --inside（被牆圍住＋頭上有頂）；露天的工作（修船灘、起重架、晾網、耙鹽）不加。
- 整柱高過 1.9 m 的機具（壓榨機、磚灶、碾盤）工具當成牆；沒有工作檯的空地（鹽田）沒分數；中殿全空的教堂 0 點 ⇒ 印格子平面圖（地板高、腰高有沒有東西）手放，另一組字母，驗站得住＋同組直線走得通。俯視圖的像素換格子容易歪，用格子圖定位。

點數照滿員（prod.js 同一套：配方 jobs × mult、公職 staff、服務菜單最多人那道 × mult）＋1：遊戲只放到點數。
匯入自檢 tools/townkit_check.cjs（townmodels.py build 最後自動跑）會報沒點、點在牆裡／海上、點比 1 級的人少。
已知沒做：地標地塊的用途是地標 id ⇒ 遊戲照它找人、沒人演 ⇒ 地標裡不放工人。
