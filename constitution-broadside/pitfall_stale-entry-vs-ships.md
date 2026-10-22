---
id: pitfall_stale-entry-vs-ships
topic: constitution-broadside
title: 「入口跟 ships/ 不是同一次建置」＝ 測試站快取的舊入口配新船檔
type: pitfall
status: active
created_at: 2026-10-23
created_by: basecamp
links: []
related_docs: []
---

「載不了：ships/<船>.js 跟這個 index.html 不是同一次建置的（清單要 X，檔是 Y）」＝ 瀏覽器手上的入口比船檔舊，不是 git 壞了（2026-10-22 TASK-0829 量的）。

- 測試站（persona9999.github.io/Blockside）index.html 是 cache-control: max-age=600；GitHub Pages 不看 `?v=<雜湊>`，舊入口要的網址照樣回新的船檔。部署前開著的分頁、或快取裡的舊入口，之後第一次用到某艘船就撞上（船是要用才載）。
- 先對帳：`git show <版>:index.html` 的 vxk-ships 清單 vs `ships/<船>.js` 尾巴的雜湊，逐版逐艘比；兩個雜湊各屬於哪一版查得出來。
- 已有自動處理：VoxelKit/voxelkit.js `staleReload` —— 對不上就重新整理一次（sessionStorage `vxk-stale:<路徑>` 記那一對，同一對再來 ⇒ 照舊報錯；file:// 不做；iframe 只重載自己）。重載後還報錯 ⇒ 真的是兩版混了（或 CDN 還沒換完）。
- ⛔ 成品零外部依賴：vxk_inline.py 擋 `fetch(`，連註解裡的字樣也擋；⛔ 不改用 XHR 繞過。
- ⚠ 改到 voxelkit.js ⇒ 指紋變 ⇒ Blocksail 預烘的城鎮網格（TASK-0823）對不上、退回現場網格化，等下次開伺服器重烘。
