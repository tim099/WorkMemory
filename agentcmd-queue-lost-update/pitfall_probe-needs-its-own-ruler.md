---
id: pitfall_probe-needs-its-own-ruler
topic: agentcmd-queue-lost-update
title: 量換檔窗口的探針必須自帶「我有沒有在換檔」那把尺
type: pitfall
status: active
created_at: 2026-09-23
created_by: kotoko
links: []
related_docs: []
---

量「換檔期間讀取端看到什麼」時，**探針必須自己帶一把「我到底有沒有在換檔」的尺**（寫入成功次數）。

🩸 2026-09-23 實測：第一版探針回 `missing = 0 / 5424`，跟被驗對象宣稱的 `0` 長得一模一樣。
而旁邊那格寫著 `writes = 0` —— PowerShell 5.1 把 `$null` 餵給 `[IO.File]::Replace` 的
backup 參數會強制成空字串 ⇒ **每一次都丟「不合法的路徑格式」，根本沒有人在換檔**。
（修法：`[NullString]::Value`。）修好後真正的讀數是 **6.72%**。

⇒ 「窗口關了」與「我的換檔一次都沒成功」在讀數上**完全同形**。
📌 所以任何報 `0` 的窗口讀數，都要並排附上**該趟的寫入成功次數與控制組**：
本題的三組是 舊寫法 46.29% ／ `File.Replace` 6.72% ／ 控制組（無寫入端）0 / 1986。

⚠ 另一格射程：`File.Exists` 在**獨佔握檔**下回 `True`（實測 `FileShare.None`）
⇒ ctypes／FileShare.None 獨佔握檔**到不了** `File.Exists` 早退那條路，
它走的是 `ReadAllText` 分支。⛔ 拿它當「爭用探針」會產出一個射程小的綠燈。
