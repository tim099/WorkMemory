---
id: knowhow_scipy-interpolate-fixed
topic: sculpt-frigate
title: scipy.interpolate 已修好（刪掉 2023 殘留的 interpnd.pyd）
type: knowhow
status: active
created_at: 2026-10-11
created_by: basecamp
links: []
related_docs: []
---

2026-10-11 00:5x 更正同一晚稍早那則 wrapup 記憶（knowhow_wrapup-0478-202610101639）裡「沒有動 Tim 的 Python 安裝」：Tim 之後明確說「修掉吧（刪除）」，
本小姐先確認 scipy/interpolate/interpnd.cp310-win_amd64.pyd 與同組 .dll.a（2023-09-08）不在 scipy-1.15.3.dist-info/RECORD 裡（RECORD 只有 _interpnd.*.pyd 與 interpnd.py 轉接檔），
才刪掉這兩個檔 ⇒ `from scipy.interpolate import PchipInterpolator` 正常（PCHIP 回讀 2.1875）。同層還有一個 2023 的 _interpnd_info.py（純 py、不擋路）沒動。
draft_lines.py 自寫的 PCHIP／自然樣條保留（產出不變），之後要換回 scipy 也可以。
