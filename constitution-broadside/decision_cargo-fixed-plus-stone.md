---
id: decision_cargo-fixed-plus-stone
topic: constitution-broadside
title: 載重 ＝ 固定壓艙＋可換壓艙、壓艙物 1 噸精度、admin.py disp
type: decision
status: active
created_at: 2026-10-26
created_by: unknown
links: []
related_docs: []
---

載重 ＝ 固定壓艙＋可換壓艙（Tim 2026-10-26 拍板；Blockside 2d97a36／9c39ebf、Blocksail d98874c）。
- ballast0（剛好浮在設計水線的壓艙物）1 噸精度（以前一格 25 噸）；T.cargoT ＝ ships[].cargoT｜設計水線排水量 × cargo[role]（merchant 0.4、war 0.15；role 沒寫 ＝ war），不超過 ballast0；固定壓艙 ＝ ballast0 − cargoT。
- 標準排水量 ＝ 滿載 − 載重（沒有貨、壓艙石也卸掉）、滿載 ＝ 設計水線；空船比水線重 ⇒ 兩個都取空船。開戰照舊浮在設計水線。
- Blocksail 貨艙 ship.ballastT ＝ 載重（舊存檔是整份 ballast0，重選一次船才換）、fixedT／stdT／fullT；battleBallastT ＝ 固定＋載重＋超出。
- 工具：Blockside python -I src/admin.py disp ⇒ reports/disp.md（量法在 js/75_shipcheck.js dispCheck：hullT0、hydrostatics、liftFor／tonsAt，跟遊戲同一套）。憲法號壓艙 0 浮高 +1.91 m 對上 32_hull.js 記的 1.9 m。
- 調空重的槓桿：ships[].material（全船預設材質）、matMap 混合比例（iron*0.79+oak*0.14）—— 莫尼特號、哨兵號照史料 1,016 → 880／884 噸、cargoT 130（變動載重 ≈ 13%：煤 100 噸＋糧水，Church《Life of John Ericsson》v2 p.8）；鐵的比例降 ⇒ 硬度、pen 一起降。
- 還沒載重的（2026-10-26）：夜隼號（空船比水線重 6.4 噸）、越嶺號、定遠號、松島號、藍鯊號（差 13 噸，水線上移約 5 cm）—— 報告列補法，等 Tim。
