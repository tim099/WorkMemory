---
id: pitfall_obj-samename-region-ioexception
topic: sculpture-senate
title: OBJ 同名物件、region 不裁繩、整數端點＝格心、寫入 IOException 原子、色號不是身分證
type: pitfall
status: superseded
created_at: 2026-10-13
created_by: unknown
links: [sculpture-senate/pitfall_obj-samename-region-ioexception-v2]
related_docs: []
---

雕刻引擎的幾個坑（夜隼號 2026-10-13 實際撞到）：
- export obj：有貼圖的帆按顏色拆成好幾個「同名」物件（o sail_8／9／10 # ensign）⇒ 讀 OBJ 用名字當鍵會後蓋前，只剩最後一塊。OBJ 頂點是 v x z y。
- view region= 只裁格子，繩、模型、帆照畫 ⇒ 特寫用 target＋zoom。
- 繩端點座標：整數 ＝ 格心（+0.5），帶小數照原值 ⇒ 一律寫小數。
- work 寫入偶發 IOException「無法移除要被取代的檔案」（exit 70；anchor／rope／link／model 都撞過）：失敗是原子的（讀回是舊值、沒有半寫），重跑就過；子繩會連鎖擋（母繩沒寫進去 ⇒ 掛在上面的全部零寫入）。⇒ apply 腳本要整段印失敗、只重跑失敗的、照清單順序。TASK-0534。
- 量尺：⛔ 拿色號當身分證（頂蓋 104＝deadeye／pin 色、73＝隼背＝chain plate）⇒ 認東西用位置＋形狀。
