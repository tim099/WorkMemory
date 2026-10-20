---
id: knowhow_replica-method
topic: town-replica-from-plans
title: 照古地圖復刻一座城：找掃描、量、比例兩把尺、擬合到世界地圖、驗
type: knowhow
status: active
created_at: 2026-10-20
created_by: gura
links: []
related_docs: []
---

照古地圖復刻一座城（金斯敦 10-19、皇家港 10-20；Blocksail tools/town_kingston_lilly.cjs、tools/town_port_royal_1692.cjs 是兩個範例）：

1. 找掃描：公有領域的古地圖常在 archive.org 的期刊掃描裡（例：Gentleman's Magazine 的摺頁圖）；大學圖書館入口常有機器人驗證，⛔ 不繞，換來源。圖片網址在瀏覽器直接開 ⇒ 同源，canvas 讀得到像素。
2. 量：疊座標格（1:1、每 50 px）讀點；規則的棋盤用投影（欄的深色比例、排的外框實線）；不規則的街區用「點種子往外填色」—— 黑的照深色、白的照框線圍起來的白（線先加粗補縫）、斜線的照深色密度；黏在一起的黑塊先往內縮 3 px 再填。
3. 比例要兩把尺對：圖上的比例尺 vs 史料的尺寸（金斯敦：Bellin 比例尺跟 50 × 150 ft 地塊差 1.32 倍 ⇒ 用地塊；皇家港：比例尺算出的岸線面積 53 英畝 vs 史料 51 英畝 ⇒ 比例尺可信）。
4. 擺到世界地圖：世界地圖的岸跟真實不一樣 ⇒ 方向照指北針固定、平移掃一片位置，條件照城的需要（皇家港：陸上 ≥ 98% 裡挑港邊外一圈最多是水的）；同分取重心，⛔ 不照自己猜的初值。
5. 產生器檔頭寫出處、量法、讀數；產物不手改；產生器跑兩次逐位元組相同；town_plan --twice、town_check --mutate、瀏覽器雜湊對拍、其他城回歸。
