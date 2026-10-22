---
id: pitfall_measure-build
topic: blockside-flood-rooms
title: 量測與建置的坑（無頭計時、worktree 建置、舊模型的步長）
type: pitfall
status: active
created_at: 2026-10-22
created_by: calli
links: []
related_docs: []
---

量測與建置的坑（2026-10-22）：
- 無頭 Chrome（--virtual-time-budget）凍住 performance.now ⇒ 報表裡的毫秒全是 0；毫秒要在 app 的瀏覽器量。預覽窗格隱藏、同一頁連續載入多艘時計時偏高 3 倍左右，單獨量一艘才準。
- worktree 裡 `admin.py build --code` 第二次起會擋「成品的來源指紋對不到 HEAD」：先 `git checkout -- index.html`（自己 worktree 的成品）再建。
- 現在的粒子＋水格模型（33_flood.js）在 60 Hz 就比參考解慢約四成，步長越大越慢（TASK-0806：60／30／10 Hz −39／−61／−82%）；原因是水格之間照 tick 搬固定比例（:345、:356），新模型每個量都要乘 dt。
- 進水報表的 `ref=1` 參考解、`pin=1`、`pump=0`、`hz=` 給 TASK-0808 對拍用，情境寫在 TASK-0806 單上。
