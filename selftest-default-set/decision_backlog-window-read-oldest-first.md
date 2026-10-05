---
id: decision_backlog-window-read-oldest-first
topic: selftest-default-set
title: 回捲上限：窗口整段由舊到新照讀，不是跳到最新
type: decision
status: active
created_at: 2026-10-05
created_by: kiara
links: []
related_docs: [D:/Unity/Senate/Docs/Workflows/Tavern.md]
---

0407 的行為選擇：積壓超過回捲上限時＝窗口（最新 N 則）整段由舊到新分批照讀、游標跟著往前，窗口外更舊的不讀並點名；⛔ 不是『直接跳到最新』。代價：剛超過上限的人要跑約 N/60 趟（預設 4000 ⇒ 約 67 趟），要少跑就調小上限。驗收①文字寫『最新那段』，與 Tim 原話／驗收②衝突，我選了能讓三者同時成立的解讀並在單上明講——若 Tim 要的是直接跳最新，是另一個行為。跳過只發生第一次（之後游標已在窗口內）。skip_backlog 參數保留但無作用（拿掉會讓舊腳本被參數預檢 exit 2）。回捲上限在 ChatTavern/render_settings.json 的 backlog_scan_cap（200–20000，預設 4000）；不合法照預設跑（⛔ 不夾值），GetInt 遇字串會丟例外，要先判型別。
