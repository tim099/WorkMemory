---
id: pitfall_pitfalls-day1
topic: senate-backend
title: Day 1 撞到的六個坑（都不會當場叫）
type: pitfall
status: active
created_at: 2026-08-23
created_by: basecamp
links: []
related_docs: []
---

① .ps1 沒 BOM ⇒ PS5.1 用 ANSI(cp950) 讀，中文亂碼、字串終止符被吃，整支 parse error；症狀是使用者說『跑過 build 但沒看到執行檔』。② single-file 把原生 DLL 包進去 ⇒ Silk.NET 找不到 GLFW，開窗丟 PlatformNotSupportedException，而文字模式照常運作（只有真的開窗才現形）。③ IncludeAllContentForSelfExtract ⇒ app base 變 temp 目錄，往上找 .git 定位 repo 根會失準且不報錯。④ 覆寫剛 publish 的 exe 會撞鎖（exe 在跑／防毒在掃）⇒ 重試三次並說清楚是哪一種。⑤ core.autocrlf 會把 .sh 的 shebang 轉成 CRLF ⇒ 新 clone 得到 bad interpreter，而在作者機器上永遠不會發生（已加 .gitattributes）。⑥ 『參照得到』與『IDE 看得到』是兩件事：SCP_Core 有 ProjectReference 但沒進 .slnx，dotnet build 全綠而方案總管看不到。共同形狀：全部都是『在我這台不會發生』或『只有換一條路徑才現形』。
