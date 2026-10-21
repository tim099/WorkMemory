---
id: decision_mixed-material-gun-hp
topic: constitution-broadside
title: 混合材質（厚度乘數＋線性混合、matMap）與砲血量公式
type: decision
status: active
created_at: 2026-10-22
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-21 拍板，已推 dev（497db00、5a06803、c1cd7ec）：
- 混合材質（05_material.js matMix）：ID 寫 `iron*0.1`、`iron*0.5+oak*0.5`（air 也可以；比例加起來 ≤ 1、沒寫滿的是空氣、第一個不能是 air）。比重、硬度、易燃、pen 線性；外觀照第一個成分。顯示名「鐵甲 50%＋橡木 50%」。
- ships[].matMap：只換這一艘遊戲裡的材質（作品不動）。莫尼特號 iron→iron*0.86+oak*0.14、哨兵號 iron*0.69+oak*0.31、定遠號 iron_hull／iron_plate。裝甲的材質檢查照 matMap 換過再比（matMapped）。
- ⛔ 坑：從材質表拿掉一個 ID 之前，先查每一艘的雕刻 meta 有沒有用它（2026-10-21 拿掉 iron_hull，松島號的防護甲板當場變橡木；ridge-001 已改掉）。
- 砲血量（10_ship.js gunHpK）：√(砲重 ÷ 2.5 t)（下限 0.2）× 彈重 ≥ 40 kg 1.5 × 砲塔層底下 4，疊乘，乘在砲種表 hpK 上。每一艘都變（以前一律 200）。開放砲盾若是砲塔層也算砲塔。
