---
id: pitfall_deck-gap-split-openings
topic: blockside-flood-rooms
title: 甲板缺口的欄托著水（上下開口要照分欄切）＋開口一多會爆掉的三處
type: pitfall
status: active
created_at: 2026-10-28
created_by: calli
links: []
related_docs: []
---

房間檔（第 4 檔）甲板缺口上的水浮在空中（Tim 2026-10-28 實測；TASK-0808 #11）：
- 成因不只「底艙上一層的甲板沒全蓋」：切房間時上下兩層的區塊重疊 < 0.3 × 較大那塊就不併 ⇒ 很多房上下疊著、中間沒有甲板（憲法號 200 間裡懸空的段 8,347 段、1,667 m²）。以前一片相連的上下接口只拆成一個開口、只接中間那一欄 ⇒ 懸在缺口上空的其他欄等於有一塊看不見的地板。
- 修法：wrsOpenings 上下的照烘焙分欄（WR_C 格）切，一欄一個開口（打穿的甲板洞 wrsDynOp 同）；?wsplit=0 ＝ 以前。量法：wrsRead 的 flt（只在讀的時候算；s.rn ＝ 那間「沒滿」的段數，0 才是滿 ⛔ 別讀反），wsimo=fltDbg 列明細。
- ⛔ 開口一多（1,790 → 10,835）三個地方會變成 O(開口數) 爆掉，殉爆沉船那局每步 15 → 119 ms（profiler 九成在 wrsPut）：
  ① wrsSemi 每打掉一格就把那一對房的開口從頭掃（＋indexOf）⇒ 改成每對房記面數、只加減差值（wrsPairAdd）；
  ② 每一欄各噴一顆粒子 ⇒ 粒子暴增、每顆落地 wrsPut 在大房間裡找 ⇒ 同一片（切之前那一整片，O.BL）這一小步的量合成一顆（wrsBlobAdd／Flush）；
  ③ s.up[r]（整間滿往上溢）每欄都放 ⇒ 沉船時每次放水掃幾百個 ⇒ 一片只放代表那一欄。
  改大結構之前先用 CDP Profiler 量（scratchpad cdp/prof.cjs：無頭但不用虛擬時間 ⇒ performance.now 是真的）。
- 副作用：浮在破口那舷高處的水沒了 ⇒ 橫傾變小 ⇒ 手感倍率 wroll 1.4 → 1.7。
- ⛔ git -C <repo> worktree add <相對路徑> ⇒ 相對的是 repo 那一層（bs_ctl_tmp 開進了共用 Blockside 裡）；一律給絕對路徑。
- 參考解（單一破口釘住）半滿 −13% 是房間內黏性 50 造成的（wvisc=0 ⇒ +3.2%），跟開口切法無關。
