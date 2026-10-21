---
id: decision_road-profile-cs
topic: blocksail-town-roads
title: 路面照 Cities: Skylines：平滑縱斷面＋挖填上限＋路口釘住
type: decision
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

路面 z 照 Cities: Skylines：平滑縱斷面、地形跟著挖填（Tim 2026-10-21 選「挖填有上限」「歷史城同一套、陡的貼地」）。
- 生成器 world/town.js finish 六步：每 2 m 量地形（細格那一圈照 S.game ＝ 遊戲那一份）→ 前後 win/2 平均 → 路口釘在先鋪的路（主要⇒街⇒小路⇒巷，同級照陣列順序）→ 縱坡兩遍夾 0.3 → 夾回 [地形−cut, 地形+fill] → 只留 > 0.1 m 的轉折。
- PROF〔佔位〕在 town.js 檔頭、匯出 BSW.TOWN_PROF；town_report 照它量挖填。數字等 Tim 在遊戲裡看了再調。
- 縱坡上限不要照道路設計給 10%：生成器找路允許到 18°，坡比上限陡就整段貼著挖填上限走 ⇒ 一條溝（托爾圖加首府 22% 的坡）。上限只負責路口坡道與夾跳動。
- Blockside 不用改：townHv 照 z 整地本來就是路寬內 ＝ 路面、兩側 1:1.5 邊坡。沒驗：遠景 16／32 m 塊會不會蓋過挖下去的路面。
