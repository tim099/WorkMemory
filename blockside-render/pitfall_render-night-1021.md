---
id: pitfall_render-night-1021
topic: blockside-render
title: render 大修 S6–S13 接手的坑：量測標籤、eve_late_high 兩態、playtest、build 拒絕、合併節奏、Refs
type: pitfall
status: active
created_at: 2026-10-21
created_by: Abigail
links: [blockside-render/knowhow_wrapup-0781-202610211020]
related_docs: []
---

接手 render 大修（S6／S8／S10–S13，2026-10-21 晚上）時會咬人的幾件事：

- 量測工具認「是不是 render 裡畫的」靠呼叫堆疊：陰影那一趟（render → shadowPass → shadowSimple → shadowCasters → drawHulls → …）已經很深，再多包一層函式，draw 就被算成 outside-render（數字照樣在，只是 by_pass 標錯）。S8 的 rbatHullR 因此把「畫掉上一段」寫在自己裡面、不叫 rbatHullEnd。
- eve_late_high 的無 MSAA 截圖會在兩個狀態之間換：同一份成品有時逐位元組相同、有時差 1369 px／max 12（S8a、S12、S11 各撞到一次，數字每次一樣）。看到這個數先別找自己的錯：draw、雜湊相同就是那一組自己的；要確認就單獨重跑。rig_torn_burn 653 px 差 1 也是老雜訊。
- playtest.py 的 menu_play 以前用 __game.flow() 判開戰 —— 0607026 之後它只記 ?start=1 分段開戰，主選單永遠 null ⇒ 一直報 battle did not start（d0e6fcd 已改成看 __game.stats() 的模擬時間與船數）。
- admin.py build --code 在工作區的 index.html 不是照 HEAD 建的時候會拒絕、成品不動（不是 exit 非 0 那種明顯的失敗）：重建前 git checkout -- index.html；grep 輸出時把「對不到」那一行也 grep 進來。快照（harness snapshot）沒帶 --force 遇到同名會跳過 —— 拿 sha1 對一下。
- dev 合得很頻繁（別人的換裝、新船、材質）：每次合回之前 git fetch dev、看共用工作區 git status 與 senate cmd coding --arg op=show；有人在場就等他推完退場再進。
- CPU 時間（ab）每輪自己晃 ±25%：S12 兩次量 gc_high 一次 +0.3、一次 −0.5 ms。只省配置的步驟，確定的讀數是配置數（Float32Array 可以包 Proxy 數；一般陣列、閉包數不到）。
- 合併訊息寫 Refs，不寫 Fixes：Fixes 會直接把單推到 done，而這批每張都有 [signer:Tim]（系統不收代簽）。Tim 驗過再 resolve（qa_note 寫他的原話）。
