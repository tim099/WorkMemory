---
id: pitfall_district-switch-clock-jitter
topic: blocksail-town-roads
title: 換城區時模擬等整圈塊、城鎮時鐘來回跳 ⇒ 鎮民原地抖
type: pitfall
status: active
created_at: 2026-10-28
created_by: unknown
links: []
related_docs: []
---

城鎮模式「鎮民不會走、原地抖，過一陣子又會動」：不是尋路。
- 鎮民位置只照 townClock ＝ SIMT ＋ loopAcc 算。
- 換城區（TOWNS.focus）時，bedWait 的中等塊那一支照焦點 bedLevels 等整圈 ⇒ 模擬停；等的時候 90_main.js 每一格把 loopAcc 歸零 ⇒ 時鐘在 0～一步之間來回跳。
- 修法：Blockside f106dac。城鎮模式焦點那一圈照載不等，模擬只等船 margin 內的塊；等的時候 loopAcc 夾在一步。
- 量法：每 50 ms 讀 townClock，數倒退與沒動的次數。
  - 一定要冷快取：同一個 origin 第二次換到同一區塊已經在快取裡，改前也量不出停 ⇒ 改前、改後各用一個沒去過的區（或換 port）。
- 改前：聖地牙哥 → 埃爾科夫雷停 4.5 秒、倒退 1 次。
