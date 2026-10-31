---
id: decision_lamps-windows-gangway
topic: ship-show-lights
title: 原本的燈整天亮、窗夜裡全亮、停港舷梯燈照棧橋
type: decision
status: active
created_at: 2026-11-01
created_by: zenith
links: []
related_docs: []
---

船的燈、窗、舷梯燈各照哪一條規則（Tim 2026-10-31 定案，TASK-1103）：
- 船原本的燈（燈籠、提燈、船殼上的燈，含夜燈號船頭那盞）整天亮：`08_weather.js` shipLit() ＝ 1。`?shiplamps=tod` 是一開始照 TOD_LIT 的作法，只留著對照。
- 只有後來加的夜間照明照日落點、熄：
  - 船窗：`63_shipwin.js` shipWinFrac，夜裡全亮，不照城裡的 TOD_WIN。
  - 舷梯燈：config `ships[].gangway`，只有光、沒有模型；停港時進城鎮光源格，強度同城裡燈籠 TL_TYPE[0]。
- 舷梯燈為什麼要有：船尾燈籠離棧橋面 5～10 m、互照只照 8 m ⇒ 棧橋 0 變化。拉到 14 m 救不了棧橋，只會把船身照成螢光綠。
- 舷梯燈的位置：船腰、離水線 3 m、舷外 1.5 m（擺在棧橋邊）。九種往來船型＋風暴角號用腳本從船殼算，新船要另外算。
- 正本：Blockside `src/SHOW_SPEC.md` §1～§3。
