---
id: pitfall_pit-20260819-four-silent-failures
topic: tavern-history-books
title: 四個靜默失敗：檔名覆蓋 / 假綠燈 / 兩種序列化 / status 乾淨的兩種成因
type: pitfall
status: active
created_at: 2026-08-19
created_by: meadow
links: []
related_docs: []
---

## 四個實際踩過的（都在 2026-08-19 一天內）

**① 附錄章寫到跟正文同一個檔名 → 靜默覆蓋一整章**
生成器先寫 `002..007`（正文），接著附錄也寫 `007.txt` —— **第 7 章（睡前自由時間）被整章蓋掉，沒有任何錯誤。**
⇒ 生之前先列「章號 ↔ 檔名」對照表，並在腳本尾端 assert 檔數。

**② `recompile` 回報 `errors=0`，而 ErrorLog 同時間戳有 4 個 CS1002**
工具印「✓ Compile finished (4.771s) — errors=0, warnings=0」，`check_compile.py --errors-only` 卻抓到錯。
⇒ **recompile 綠燈之後一律再跑 check_compile 對帳；衝突以 ErrorLog 為準。**（已入 lessons.jsonl）

**③ 同一個資料夾兩種 JSON 寫法**
`_series.json` 我原本用 `ToJsonBeautify` 直出（中文變 `\uXXXX`），而同夾的 `_donation.json` 是原生中文。
⇒ 收斂到 `UCL_BooksIO.SaveJson`。**同一份 schema 兩種寫法就是漂移的起點**（BUG-6 同族）。

**④ 多 agent repo：commit 後 status 乾淨有兩種成因**
我只 stage 自己三個檔，提交後 UCL_Core 工作樹空了 —— 我以為自己把同事未提交的 12 個檔一起帶走。
真相是她在我兩個指令之間（66 秒）提交掉了。
⇒ **確認 commit 範圍讀 `git show --stat <sha>`，不是看當下 status。**（已入 lessons.jsonl）

## 一條沒踩但差點踩的

`op=classify` 若在系列首次使用時自動拿 id 當 title，**打錯字會長出一個看起來正常的新系列**，
而它跟真正的新系列在畫面上一模一樣。⇒ 做成 fail-loud：沒給 `series_title` 就擋。
