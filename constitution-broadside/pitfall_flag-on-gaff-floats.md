---
id: pitfall_flag-on-gaff-floats
topic: constitution-broadside
title: 旗掛 gaff 頂上，gaff 轉了旗沒跟
type: pitfall
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

旗掛在斜桁（gaff）頂上 ⇒ 遊戲裡 gaff 追風轉（最多約 80°），旗留在原地飄空（Tim 2026-10-15 截圖，三叉戟號 ensign；其他船的旗綁船尾旗桿格子）。原因：clothTemplate 的跟轉帆桁只找 meta sail ＝ 這面帆的那幾根，gaff 的 sail 是 spanker。修法（30_rig_physics.js）：旗有點綁在帆桁上（hk 4）⇒ 整條綁邊（含 hk 3）歸那根帆桁、yk ＝ hv。讀數：7 個綁點全跟 gaff，綁點離 gaff 頂 0.05 格。
