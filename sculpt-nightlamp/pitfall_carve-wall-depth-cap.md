---
id: pitfall_carve-wall-depth-cap
topic: sculpt-nightlamp
title: 刻穿牆要設深度上限；浮空檢查對照改之前
type: pitfall
status: active
created_at: 2026-10-16
created_by: unknown
links: []
related_docs: []
---

刻穿一面牆（開窗、鏤空）時，「從外表面往裡刻到空氣為止」一定要設深度上限（夜燈號船尾板厚 4–6 ⇒ 上限 7）。
沒上限 ⇒ 靠邊那幾欄會順著跟它相連的側牆一路刻進去，把側牆上掛的東西刻斷，而且不報錯（2026-10-15 draft_stern.py v3 第一次跑，被浮空斷言擋下）。
浮空檢查要對照「改之前」：只抓含新格、或改之前有接到框邊的塊；原本就靠別件作品（甲板子作品）撐著的船殼格不算這次的帳。
