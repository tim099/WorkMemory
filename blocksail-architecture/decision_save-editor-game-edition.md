---
id: decision_save-editor-game-edition
topic: blocksail-architecture
title: 存檔兩種同格式、一城一資料夾、分區存；Build／Editor 旗標與城鎮建造權限表
type: decision
status: active
created_at: 2026-10-24
created_by: unknown
links: []
related_docs: []
---

Tim 2026-10-24 一路拍板的存檔與版本旗標（TASK-0873、0875；權威文件 Blocksail docs/PACKAGING.md §9、§10）：
- 存檔兩種、同一個格式（world/save.js savePack／saveUnpack）：Editor 版本 ＝ repo 的 world/start/（打包進遊戲、新遊戲從它開始）；遊戲存檔 ＝ %APPDATA%\Blocksail\saves\<名字>\（很多份）。
- 一份存檔 ＝ 一個資料夾，一城一個子資料夾（Tim「檔案請盡可能拆開」）：save.json、ledger.json、families/<家族>.json、towns/<城>/state|ops|buildings|stores.json。
- 存成 Editor 版本是另一顆鈕、分區存（世界／每座城）；城鎮編輯模式的 💾 只存那一座城（Blockside 87_townbuild twbSave ⇒ mapgen serve townSave）。按了才存，不自動存。
- 版本旗標 BSW.EDITION：打包（copy_game.cjs）定成 Build、切不回；repo 開是 Editor，主頁可切去測 Build。只給 Editor：島嶼預覽、城鎮規劃、存到 Editor 版本、#stat；畫範圍兩種都留。
- 城鎮建造權限表 BSW.townCan：Editor 全部（改城鎮預設、拆所有、改配方、不用買）；Build 只能動自己的、之後要花錢。閘在 mapgen.js townEdit，不只藏按鈕。拆除、改配方、花錢買還沒有這些操作。
