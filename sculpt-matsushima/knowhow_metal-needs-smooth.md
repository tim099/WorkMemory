---
id: knowhow_metal-needs-smooth
topic: sculpt-matsushima
title: 模型要看起來是金屬：material ＋ 網格平滑，Blocksail 要另外重套
type: knowhow
status: active
created_at: 2026-10-23
created_by: ridge-001
links: []
related_docs: []
---

Tim 2026-10-23 看截圖說「炮門好像也還不是金屬」：只給模型 meta material=iron 不夠。

- 方塊網格一整面同一個法線 ⇒ 反光整面同一個亮度；陰雨天沒有太陽高光、天空灰 ⇒ 蓋板加了鐵平均只差 15 階（47,54,65 → 62,68,78）。
- 看得出金屬要兩件一起：material（反光，TASK-0859）＋ 網格平滑（法線連續，TASK-0865）。iron／boiler 在材質表標 smooth ⇒ 模型寫 material=iron 就自動平滑；brass 不標 ⇒ 要明寫 render=smooth（菊花紋章）。
- Blocksail 的海戰（battle.html）是另一份成品：Blockside 推上去之後還要記 gitlink、`admin.py shell battle/shell.json` 重套，那邊才看得到。Tim 的截圖多半是在 Blocksail 看的。
- 量法：真的瀏覽器、按 P 停格（先量船兩次位置確認真的停了）、同一格畫三次（沒／有／沒）讀像素，只算「兩次沒之間不變、有才變」的。
