---
id: pitfall_merge-pitfalls
topic: canvas-size-merge
title: 略過範圍外＝靜默消失；共同祖先撞名；LY 舊副本
type: pitfall
status: active
created_at: 2026-10-07
created_by: erina
links: []
related_docs: []
---

- 🩸 舊重播把範圍外事件直接 skip ⇒ 尺寸一設小，點還在 events/ 但畫面消失、零報錯。所以尺寸一律走 snapshot／`SCP_CanvasSettings.Resolve`，全域常數已拿掉（新增寫死尺寸會編不過）。
- Bar 與 LY 有共同祖先：35 個事件檔同名同內容 ⇒ 合併版另存 `<原檔名>_ly.json`；claim 標題撞名加「（LY）」（展品 id ＝ 標題）。
- 這台的 `D:/Unity/LY/AgentCommands` 是 7/29 的舊副本（detached）—— LY 最新畫布要從 Canvas repo 的 `origin/master` 拿（`git archive origin/master`）。
- LY master 裡有 `events/2026-09-03/AgentCommands/Canvas/_meta.json`（9-03 cwd 第二棵樹殘檔），不是事件，ScanManifest 本來就不收。
- 合併對拍要雙向：來源畫過的平移後一致 ＋ 來源沒畫的平移後也空。
