---
id: pitfall_new-good-needs-start-row
topic: blocksail-production
title: 新貨物不會自己長出來：開場特產列＋消費端
type: pitfall
status: active
created_at: 2026-11-01
created_by: unknown
links: []
related_docs: []
---

新加一種貨（goods.js＋配方＋建築）之後，自動建築**不會自己蓋第一座**：市場上沒有這種貨 ⇒ 沒有需求 ⇒ 候選的估值是 0（TASK-1073：椰子、海龜、桃花心木、焦油一年都是 0；tools/auto_trace.cjs 量的，皇家港、哈瓦那 66～70 輪每輪都是 0）。香蕉例外：1 級主糧吃到「缺糧」那一份估值。
⇒ 要它在遊戲裡出現：開場檔放一座鎖住配方的建築（towns/<城>/ops.json 的 auto ＋ buildings.json 那一列＋save.json files），照 world/auto.js candidates 的算法算 cost／mat（session 5065a8ce scratchpad sjtools/auto_rows.cjs；need.suit 要自己補進 site.suits）。
吃它的那一頭也要有人：桃花心木要同時放一座做桃花心木家具的木工坊；焦油的消費接在船廠的修船（ship-repair-tar，船具缺貨時 SWITCH 換過去）。
物種條件：tools/land_start.cjs --write 1 寫 land.species／suits；海洋物種（綠蠵龜）從 TASK-1073 起照海上的點數（以前只數陸上 ⇒ 永遠不在清單）。寫完逐座比：拿掉新的那幾個，其他要一字不差。
