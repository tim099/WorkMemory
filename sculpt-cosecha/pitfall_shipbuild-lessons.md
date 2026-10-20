---
id: pitfall_shipbuild-lessons
topic: sculpt-cosecha
title: 造收穫號踩過的坑（下一艘船照樣會踩）
type: pitfall
status: active
created_at: 2026-10-19
created_by: calli
links: []
related_docs: []
---

造收穫號踩過、下一艘船照樣會踩的（細節在作品 notes.md）：

1. 設計程式讀「船」要扣掉本階段自己的件 —— 不然重產時把自己上一輪的東西當成地形（draft_deco、draft_masts、draft_gear 都有扣）。
2. 沿用別人的砲：砲管頂比 1618 法令的門楣高一格 ⇒ 砲口伸不出去；甲板舷弧是台階 ⇒ 門檻照「砲腳那一段甲板最高處」量（layout.ports ref_pad）。
3. 量砲口在船殼外：只看砲口往外那條線 ⇒ 砲門是洞、量不到牆；改量砲門兩側的船殼面。修好的尺讀 0，先餵一個一定會錯的東西（往裡拉 8 格）。
4. 遊戲 Blockside 40_crew.js blocked() 用模型的軸對齊外框當佔位 ⇒ 轉 45° 的錨擋住旁邊砲手；tools/crew_sim.py 照遊戲規則模擬（砲班站位＋桅頂 topSpot），跟 shipcheck 對得上。
5. 桅頂水手：遊戲在桅軸半徑 9／12／15／18 找站位、一層 6 點 ⇒ cofa 至少 r 12；平台要在 shrouds 綁點（hound）上面。
6. 抄來的程式在原本的地方沒被逼到角落過（夜燈號的火藥桶只有一層 ⇒ 兩層時立柱插進桶、架板浮空、兩舷不對稱）。
7. 匯出 vxk：草稿要帶 draft=1 才進匯出；甲板子作品給 decks.py 要帶 links=none。
8. Blockside 共用工作樹常有別人沒提交的改動 ⇒ 匯入／改 code 一律在暫存副本或自己的 worktree（git archive HEAD＋複製 _cache）。
