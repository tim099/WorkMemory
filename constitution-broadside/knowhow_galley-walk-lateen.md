---
id: knowhow_galley-walk-lateen
topic: constitution-broadside
title: 槳帆船：甲板 meta walk（中央走道）與三角帆桁 rig=lateen
type: knowhow
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

槳帆船上船的兩個坑（皇家號 2026-10-18；造船規格 §5、§9.3 已寫）：
- 走路格只看甲板 floor 上下 −2～+3 格，而槳帆船人走的是比地板高 0.9 m 的中央走道 ⇒ 甲板被划手座切成三十幾塊、送藥手全斷（0／19）。⛔ 直接把 floor 改成走道高：判層照 floor（地板上 −3～+20），甲板面上的東西會被判到下一層。⇒ 甲板 meta 新鍵 walk（同 floor 寫法，走路格照它、其他照 floor；35_decks.js deckWalk）。量法：照遊戲 navZ／navStep 在 Python 算連通，改之前不通、改之後通當反向對照（design/draft_cabins.py）。
- 三角帆桁只標 yard=gaff ⇒ 照 gaff 往下風擺到 80°，主帆桁前端甩出舷外 13 m、tack 浮在海上。⇒ 帆桁 meta rig=lateen＋固定點 fore ⇒ 最多擺 asin(2.5 m ÷ 轉軸到 fore 的水平距離)（30_rig_physics.js yardSwingMax）。持火號（erina）也是三角帆，已在酒館提醒。
