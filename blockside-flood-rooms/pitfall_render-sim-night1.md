---
id: pitfall_render-sim-night1
topic: blockside-flood-rooms
title: 房間水面與房間模擬第一階段的坑：return 換行、舊快取、漣漪上限、粒子落進灌滿的空腔
type: pitfall
status: active
created_at: 2026-10-22
created_by: calli
links: []
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/33_wrsim.js, D:/Unity/Blocksail/external/Blockside/src/js/63_wrwater_gl.js]
---

畫房間水面、房間模擬第一階段踩到的坑（2026-10-22 夜，calli/flood-waves）：
- `function f() { return` 後面直接換行再接字串 ⇒ JS 自動補分號、回 undefined；建置（acorn 語法檢查）照樣過，執行時才在 VK.compile 炸（`reading 'split'`）。⇒ return 後面同一行要有東西（`return '' +`）。
- 瀏覽器會留舊的 index.html：重建之後同一個網址重開可能還是舊版 ⇒ 網址多帶一個沒用的參數（&nc=N），並且讀頁面上的來源指紋確認。
- 漣漪點有上限（24）時，每一格畫面能起好幾圈 ⇒ 上限一下就輪完、每圈活不到半秒、畫面上看不出來；讀數上「24 圈全是剛起的」就是這個 ⇒ 整艘限速（0.12 秒一圈）。
- 房間模擬的粒子：落到灌滿的小空腔（艙底 2–5 m³ 那幾間）會被判成「進到水面以下」⇒ 放進一間滿的房 ⇒ 溢出去當成流回海裡（48 秒漏了 123 m³）。
  修法：那一欄滿到頂 ⇒ 當成打在它頂上的水面（落點 ＝ 前一格、上面那一間）；放不下 ⇒ 用落點的高度當水頭往上溢 → 正上方另一間 → 放回出發的那一欄。修完 48 秒漏 0.21 m³。
  ⛔ 落地時給的水頭 −1e9（不准往上溢）也是同一個洞的一半。
- 分類計數（s.dbg：落進水裡／撞實心／溢出去／找不到房間／放回）比看總數有用：一眼看出是「溢出去」而且集中在第 0、7、10 間。
