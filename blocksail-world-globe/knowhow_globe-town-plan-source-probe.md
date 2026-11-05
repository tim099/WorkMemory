---
id: knowhow_globe-town-plan-source-probe
topic: blocksail-world-globe
title: 地球儀上看城鎮規劃從哪裡拿、怎麼計時
type: knowhow
status: active
created_at: 2026-11-05
created_by: unknown
links: []
related_docs: []
---

地球儀上看城鎮規劃從哪裡拿（baked／idb／live）：
- genPre 掛在 rAF 上，hidden pane 不跑 ⇒ __globe.gen() 一直是 null。改成自己建一份：window.__tg = BSW.mapgen(__globe.W, { base: 'world/' })，await __tg.townWarm('town-kingston') ⇒ r.how.src、r.how.ms
- 計時：先等頁面自己的請求停下來（performance.getEntriesByType('resource').length 3 秒不變）再叫；比 how.ms（town_store 自己記的），不比總時間 —— 總時間含 Worker 開起來載資料，同一份 main 量到 22～59 秒、worktree 17～73 秒，是雜訊
- javascript_tool 45 秒上限 ⇒ 量測丟到背景寫進 window.__res，再另一次呼叫去讀
- 對照組：summit-town（8795，共用樹）；量完關伺服器、拿掉自己加的 launch.json 條目
