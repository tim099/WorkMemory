---
id: knowhow_burn-douse-controlled-test
topic: constitution-broadside
title: 著火撲不滅的原因、救火修法與受控實驗的量法
type: knowhow
status: active
created_at: 2026-10-19
created_by: erina
links: []
related_docs: []
---

著火撲不滅的原因與受控實驗（2026-10-19，Blockside 014a6db）。

- 效能：著火本身不是瓶頸（燒到上限 6000 格延燒每步 0.075 ms，著火全部不到模擬的 5%；早就是每格分 15 步、每艘錯開）。真的卡要看畫面那側（粒子、火光、換色重建網格），得在 app 瀏覽器、視窗開著時量。
- 撲不滅：預備水手從艙底爬上來，點火 30 秒沒人到；24 人全到一秒潑 360、火一秒長 600，潑過的地方照樣往外點 ⇒ 過三千格就沒救。
- 修法：潑濕（火場 wet 壓延燒與重新點著）、內建消防泵＋玩家擺的抽水機接 20 m 水帶、甲板上的砲手先去（遠的收回來改派）。
- 受控實驗（不受戰局影響，建議照這個量救火）：?start=1&mode=eve&ship=constitution,ember&seed=3&dist=3000&debug=1，開場 5 秒用 __game.burnAt 在主甲板中段點半徑 10 格（692 格），每 10 秒讀 __game.burn(0)。
  dev 7269e17：最大 1504 格、75–80 秒熄、燒穿 1092；修完：1184 格、40 秒熄、燒穿 141；擺兩台人力抽水機（?pumps=2,0）：35–40 秒、燒穿 0。
- 持火號、皇家號 ships[].fireEngine false（槳帆船沒有內建消防泵）。
