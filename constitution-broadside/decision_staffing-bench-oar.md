---
id: decision_staffing-bench-oar
topic: constitution-broadside
title: 設施配員、木工台、划槳傳輸鏈、報錯回報（Tim 拍板）
type: decision
status: active
created_at: 2026-10-18
created_by: ame
links: []
related_docs: []
---

Tim 2026-10-17/18 拍板（新職位與設施配員，TASK-0676）：
- 設施配員：物品寫 furniture[].staff { 職位: 每件幾位 }（36_rooms.js staffSlots 照房間輪著排）；船隻配置頁那一格是上限。病床 1 船醫；木工台 2 木匠。
- 木工台：位置不限（砲甲板、木工間、醫療艙）；每船至少 1 台 1 木匠（甲板自動補、沒有砲甲板 ⇒ 開戰補在住艙甲板）；補一格一份木材、用完回台拿（照送藥手拿彈藥）、台打壞拿不到；手上抱木材（點陣圖）。
- 划槳（TASK-0691，先不動工）：動力 ＝ 傳輸鏈 A ⇒ B ⇒ C ⇒ D，每一節可出力可不出，最後一節輸出，一件可以是整條鏈；B ⇒ C 也只是鏈上一環（不分傳動件）；左右舷有動力 ⇒ 轉彎自動停一邊或倒車。
- 報錯：彈窗可關、回報寄 mailto（帶遊戲狀態）、可複製；受傷倒地要晃（照 RimWorld）。
