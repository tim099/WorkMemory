---
id: pitfall_scp-gui-explicit-key-and-reload
topic: unity-senate-migration
title: SCP_Gui 頁：explicit key 不吃 IdScope、迭代中途別 Load、下拉要世代號
type: pitfall
status: active
created_at: 2026-10-01
created_by: basecamp
links: []
related_docs: []
---

寫 SCP_Gui 頁（immediate mode）踩到的三格（2026-10-01，SCP_GuiPlurkPage）：
1. **explicit key 不吃 `IdScope`**：迴圈裡每列 `g.Dropdown(..., "pick")` ⇒ 全部同名，只靠出現順序加 `#2 #3` 區分 ⇒ 名單一變，選單就對到別人那一列。⇒ key 自己帶列的身分（`plurk/p/<persona>/pick`）。
2. **寫入後不要在 foreach 中途 `Load()`**（重建正在迭代的清單 ⇒ `InvalidOperationException`，而寫入其實成功了）⇒ 迴圈裡只記「要做什麼」，跑完再做。
3. **下拉值存在 host 欄位裡** ⇒ 寫入失敗時選單停在新值≠現況 ⇒ 每一格重送。⇒ 加世代號（寫入嘗試後 key 換一代，選單回到現況）。
另：`senate ui --set <id>=<值>` **選不了下拉**（那個 id 是展開鈕）；要 `--click <key>` 展開再 `--click <key>/pick/<值>`。⚠ `ui --json` 不帶 `--local` 會由常駐視窗（線上舊版）回答，不是你剛 build 的那顆。
