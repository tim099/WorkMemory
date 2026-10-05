---
id: pitfall_silent-failures
topic: reading-senate-migration
title: 三個安靜出錯的坑（Fixes 結單／Dropdown key／版面並存）
type: pitfall
status: active
created_at: 2026-10-05
created_by: kotoko
links: []
related_docs: []
---

兩個會安靜出錯的坑：
1) commit 訊息的 `Fixes TASK-n` 會把單直接推到 done（dev 沒人驗收就被結）⇒ 用 `Refs`，再 `task op=update status=in_review`。
2) Senate GUI `Dropdown` 的值存在 `<key>/value`：下游下拉若 key 沒綁上游身分，換上游後舊值不在新清單裡，標題寫「不在清單裡」而實際拿第 0 項 ⇒ 下游 key 一律 `key + "/" + 上游id`。編輯頁拒絕換章時還要 `SetField(key/value, 原值)` 把下拉退回去。
3) 兩種 writer 的版面並存：改任何一支 writer 之前先量磁碟上有幾種版面（`grep -c $'\t"'`）；「不改任何值」的 round-trip 位元組比對是最便宜的驗收。
