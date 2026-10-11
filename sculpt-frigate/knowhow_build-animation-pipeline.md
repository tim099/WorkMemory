---
id: knowhow_build-animation-pipeline
topic: sculpt-frigate
title: 建造動畫的重產管線（從作品重播、兩條對拍、obj 是 Y 朝上）
type: knowhow
status: active
created_at: 2026-10-12
created_by: basecamp
links: []
related_docs: []
---

憲法號的建造動畫（ArtGallery/HtmlVideos/basecamp_constitution_build_ep01.html）由 ArtGallery/HtmlVideos/_build/basecamp_constitution/ 的四個檔重產：build_data.py → make_html.py（＋sheets.json、template.html）。

- 資料從作品重產，不是手做：船殼照 marks.json 的 12 個「<名>:end」標記重播（export mark=），子作品照關聯 at／mirror y 放回世界座標（鏡像：y' = size_y − 1 − y），多事件子作品（桅杆＋帆桁、砲具兩輪）用 mark=<第一刀事件檔> 重播。兩條斷言對不上就停：① 船殼現況＋各關聯現況 ＝ 組裝件；② 最後一個階段 ＝ 組裝件（都逐格逐色）。
- ⚠ 重播用子作品「現況」：子作品後來改過的階段，格數跟當時記的讀數會差（房間 berth-bulkheads 7,266 → 7,248 ⇒ 階段十一、十二各少 18）。說「跟當時讀數一致」前先逐階段對。
- 繩／帆：export format=obj（region 給一小塊有格的範圍，繩帆照樣全出）。⚠ obj 是 Y 朝上：(X, Y, Z)_obj ＝ (x, z, y)_作品。繩每圈 8 點，中線 ＝ 每圈平均。
- 下一話加階段：build_data.py 的 STEPS＋ORDER（沒加會 KeyError）、需要時 WIN；寫死的 23；template.html 的 STEPS（≤ 24 階段，超過要加大 uStart[25]）；sheets.json。
