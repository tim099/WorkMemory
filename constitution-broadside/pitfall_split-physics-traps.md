---
id: pitfall_split-physics-traps
topic: constitution-broadside
title: 拆檔與物理的坑（template 不能直接開、檢查掃註解、-I 忽略編碼、物理不抽亂數、布看約束數）
type: pitfall
status: active
created_at: 2026-10-12
created_by: ame
links: []
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-12 晚上踩到的：
- **template.html 不能直接開**：`.vxk` 要 `vxk_inline.py` 內嵌才載得起來（直接開 ⇒「載入失敗：Failed to fetch」）。量測一律開建好的 `HtmlGames/ame_constitution_broadside.html`。
- **`vxk_inline.py` 的外部依賴檢查會掃 JS 內容**：註解裡寫了字面上的 `<script src=…>` 也會被擋（未寫入）—— 註解別寫標籤原文。
- **`python -I` 會忽略 `PYTHONIOENCODING`**：印中文或 ✓ 在 cp950 主控台會 UnicodeEncodeError；腳本裡用 `sys.stdout.reconfigure(encoding='utf-8')`。
- **拆成 `js/` 之後仍是同一個作用域**：新的頂層名字先搜一遍有沒有撞名；頂層敘述照檔案順序執行。
- **物理不准抽 `rnd()`**：開或關要走出同一局；`dropSail` 的亂數照抽、只是布還在時不撒。改物理後用同一個種子對拍（`__game.run(30)` 的 stats 與 `__game.cloth()`）。
- **布的負擔看約束數**：每個約束 ~12 ns，換 Float64Array 只快 15%；要省就縮格（粗格內插），不是微調迴圈。
- 截圖要量瞄準圈這種小東西：先送 `p` 暫停；`zoom` 每次會重截一張。
