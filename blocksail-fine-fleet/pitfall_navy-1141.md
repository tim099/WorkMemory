---
id: pitfall_navy-1141
topic: blocksail-fine-fleet
title: 國家的戰艦：疊乘失控、走航道圖、測法
type: pitfall
status: active
created_at: 2026-11-01
created_by: calli
links: []
related_docs: []
---

**國家的戰艦（TASK-1141）的坑與量法**（calli 2026-10-31）
- 效果疊乘會失控：現在只有一塊海盜區（向風海峽），每國戰艦都擠進去；第一版 sup 0.5、不封頂、上限只照人口 ⇒ 10 艘、同時 6 艘在巡 ⇒ 0.5⁶，被搶的貨少 97%。改成每國每區最多 perZone 2 艘、每艘 × 0.7、一區最多打到 0.4 折 ⇒ 少一半左右。之後加海盜區要重量。
- 戰艦要走航道圖（fleetfine.js steer，同 TASK-1140 的海盜船）；直線 toward 會穿島。圖上到不了 ⇒ navyStuck 當作到了。
- 戰艦 S.fleets.<國>-navy-<n> 沒有 ai、沒有 crew：所有照 F.ai 篩的地方都自動跳過；地球儀 fleetText／fleetName 要另外處理 kind 'navy'（不然 A.from 當掉）。
- 精細層要在地球儀測：G.clock 跳天也會跑精細小時，但海盜船在港裡一待 14～42 天；要看追擊就把 pir.left 撥 1。__globe.ship 是函式，要 G.ship() 拿物件（寫到函式上不會報錯，只會進不了城）。
- 量法：ntn_probe.cjs／ncmp.cjs（session 59d88935 scratchpad n/）記每國國庫、戰艦支出、被搶的貨（rep.fleet.raid 的 took）、海盜口袋；反向對照 NAVY.on=false。
