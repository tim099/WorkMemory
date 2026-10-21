---
id: pointer_external-shell
topic: constitution-broadside
title: 外掛外殼 admin.py shell 與 Blocksail battle.html 的重套規矩
type: pointer
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

外掛外殼（2026-10-21，Blockside c087d25；Tim「直接做一版Blocksail的海戰頁面 只是邏輯共用Blockside」）：

- `python -I src/admin.py shell <外殼設定.json> [--check]`：拿建好的拆檔成品（入口；船檔共用）套別的遊戲的外殼寫成另一頁。不重走建置、不要 _cache。
- 核心行號一行都不動：設定一行塞在 </head> 前（window.BS_SHELL：ships 船檔前綴、name、stamp），外殼的 js／css 接在 </body> 前。
- 核心的掛鉤只走 window.BS_SHELL：VoxelKit/voxelkit.js 的 ships 前綴、71_input.js shellCall('menu')。要加新掛鉤 ⇒ 核心裡用 shellCall，外殼回 false ＝ 照 Blockside 的。
- Blocksail 的 battle.html ＝ external/Blockside/index.html 套 battle/（shell.json、shell.js、shell.css）。⛔ Blockside 每換一版、或改了 battle/ ⇒ battle.html 就舊了：
  在 Blocksail 根目錄 `python -I external/Blockside/src/admin.py shell battle/shell.json`、跟 gitlink bump 同一個提交（ROADMAP 規矩 2）。tools/world_serve.py 開機會 --check 提醒。
- 主選單那張卡在 battle.html 預設藏（帶 start／pick 開時不閃 Blockside 的主選單）；不帶的（主頁「出航」）照常顯示。
- 第 2 步（Blockside 自己的主選單、設定頁也抽成外殼）：規格在 Blocksail spec/core-shell 分支 docs/CORE_SHELL.md、TASK-0774，等 Tim 答 §7。
