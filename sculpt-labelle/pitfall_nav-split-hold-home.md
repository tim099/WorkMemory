---
id: pitfall_nav-split-hold-home
topic: sculpt-labelle
title: 匯入檢查 ✗ 0 不等於走得到：主甲板走路格切成三塊、貨艙梯頭落在孤島
type: pitfall
status: active
created_at: 2026-10-26
created_by: unknown
links: []
related_docs: []
---

匯入檢查 ✗ 0 不等於預備水手走得到：拉貝爾號 v1 的主甲板走路格被切成三塊，匯入檢查沒報。

- 待命那一層 ＝ 最低那座梯子的 lo（Blockside 42_hands.js：home = min ladder.lo）⇒ 有貨艙梯子的船，預備水手待在貨艙。
- 拉貝爾號：貨艙梯子的梯頭（x 163）落在主艙口的井與前艙口之間那一小塊，兩舷又是第 3 門砲的外框（從 x 163.3 起）⇒ 從貨艙上來哪裡都去不了；廚房「四個面都沒有連得到待命那一層的走路點」才露餡。
- 量法：頁面 ?shipcheck=<船>&debug=1，__game.ships() 找那艘，讀 sh.nav.levels[i].N（ok、reg、Z、blk；x0 起每 2 格一點、gx 奇數 gy 偶數）與 sh.nav.ladders[k].ra／rb（梯子兩頭接到哪一塊）。主甲板 reg 不只一塊 ⇒ 看哪座梯子接哪一塊。
- 走路點只看那一根格柱（navZ：地板 −2～+3 格裡、頭上 12 格空、不在模型外框裡；門 kind=door 不擋也不當地板）⇒ 補連通：井前端兩角留成甲板（draft_decks well_keep_fwd）、前艙口改成封死走得過去的蓋子（頂要在地板 +3 格內）。
- 改完：主甲板 1 塊、火藥 12／14 → 14／14、戰局開砲 34 → 114 發（單一種子）。
