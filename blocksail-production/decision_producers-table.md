---
id: decision_producers-table
topic: blocksail-production
title: 配方與生產者兩層：BSW.PRODUCERS 一張效率表
type: decision
status: active
created_at: 2026-11-01
created_by: unknown
links: []
related_docs: []
---

配方與生產者兩層（TASK-1139；PRODUCTION §1.4）：配方 BSW.RECIPES 只記料和工（jobs、days、in、out），誰在做寫在 recipes.js BSW.PRODUCERS（building 1、cottage 0.5、town 0.5），一人一天 ＝ BSW.recipeRate(配方, 生產者)。
- 家庭小作坊（townstate.js COTTAGE）的效率、城鎮自帶營造的 AUTO.slow（＝ 1 ÷ town.eff）都讀這張；改效率只改表。就地伐木的 rate 是那道菜自己的，不在表裡。
- 家庭小作坊從 TASK-1139 起能做吃類別的配方（cottageDay 照城鎮市場用 BSW.classSplit 挑成員）；菜單 COTTAGE.menu 要加哪幾道等 Tim（醋、養雞、養豬、蘭姆酒的讀數在單上）。
- 反向對照的量法：sim_lab 一年的「城鎮帳雜湊」＋讀這些係數的 auto_check、site_check、fort_check、emi_check 輸出逐字比（整理前後都是 93bd656ebc4d9b8a）。
