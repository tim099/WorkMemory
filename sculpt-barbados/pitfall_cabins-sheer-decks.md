---
id: pitfall_cabins-sheer-decks
topic: sculpt-barbados
title: 有舷弧的甲板隔空艙房：鋪平地板、標 .door、用遊戲 roomPlans 驗擺不擺得下
type: pitfall
status: active
created_at: 2026-11-02
created_by: apex-two
links: []
related_docs: [D:/Unity/Senate/Docs/Workflows/Ship_Decks.md]
---

巴貝多商人號（TASK-1147）隔空艙房學到的，下一艘有舷弧的船照這個做：
- 甲板有舷弧（後甲板底下 x 每 1 m 降 1 格）⇒ rooms.py 的框要「地板一樣高、框裡全空氣」，長過 1 m 的房一定跨台階 ⇒ 每間在艙房子作品裡鋪一層平地板，鋪到框裡最高那一柱；門口台階 ≤ NAV_DZ 3 格（42_hands.js），差太多就在門外補台階。
- 地板比門外高 ⇒ rooms.py 的門洞量不到（它拿框的 z0 去量門外那幾格）⇒ 每間標 `<房名>.door`，check 才不報「找不到門」。
- 框的寬：船殼往內收（tumblehome）那幾排天花板低 ⇒ 留著會把整個長方體的頂壓低（左舷艙 v1 只剩 1.0 m 高）⇒ 邊上那排頭上不到 1.2 m 就往內縮。樑底下照實壓低框頂（框要全空氣）。
- rooms.py check 只驗幾何；擺不擺得下要用遊戲自己的 `__game.roomPlans('<id>', 'medical'|'workshop')`（?debug=1，看 auto 欄）。水手長艙 v1 rooms.py 全 ✓、遊戲卻「放不下必要件：病床」（能擺的只有 1.5 × 2.0 m，而且門開在短邊、3 排格子門口那排兩旁都留走道）。
- 挑房間的位置先避開：廚房爐灶四面的廚師站位（外框外 2 格）、梯子上來的落腳處、絞盤四周；船樓太矮（1.6 m 以下）鋪平就不到 1.4 m ⇒ 不隔。
- 產生程式：作品 design/draft_cabins.py（plan.cabins_build；--probe 只印讀數）、tools/build_cabins.py；出處 abigail-fluyt-01m draft_fit.py。
- ⚠ reimport 讀的是 Sculpture 作品現在的樣子 ⇒ 別人還沒提交的 meta（這次是 summit TASK-1169 的 kind=mast）會一起進 vxk；提交 Sculpture 時只 add 自己的路徑、合 dev 前先跟對方講好誰重匯。
