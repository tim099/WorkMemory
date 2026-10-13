---
id: state_wip-2026-10-13b
topic: constitution-broadside
title: 2026-10-13 早上：TASK-0520 設定存檔＋船型快取已提交（748f279），剩手機量一次；仍等 basecamp 餘燼號的砲
type: state
status: superseded
created_at: 2026-10-13
created_by: ame
links: [constitution-broadside/state_wip-2026-10-13, constitution-broadside/state_wip-2026-10-13c]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/README.md]
---

2026-10-13 早上（wake #14，Florin）：TASK-0520 設定存檔＋船型快取已提交 ArtGallery 748f279（單層；父層 Valhalla 還指著舊 hash、沒 push）。沒有東西在施工。

TASK-0520：in_progress，驗收 8 格勾 7；剩第 6 格的「手機量一次」—— 手機開兩次，第二次載入那一行寫「讀快取 N 秒」，讀數貼回單子就能結。PC：現場 1.24 秒 ⇒ 讀快取 0.07 秒。讀數與做法在單子留言 #1。

還在等別人的（照舊）：basecamp 做餘燼號的砲（TASK-0519，todo；她在等 Tim 選下一個建造階段）。砲好了之後（ame）：正式匯出 basecamp-ember-01m-assembly（format=vxk include=rig,models,parts,meta）→ template 加 ship-ember 的 x-vxk 標籤（HULL_KNOWN 已有那一行）→ 對 32 磅長砲數值、三層甲板站位與取用點、彈藥庫 → 重量整場 → 展品卡寫兩艘船。
舊的線照留：course 下角被帆桁整面拖著轉、頂風失速／換舷、美術三張圖（TASK-0516）。

對拍：?demo=75&still=1&cam=deck 現在 sha256 51b2d06b…（這次沒變）。改程式後建置前先確認 _cache/constitution.vxk ＝ 版控成品內嵌的那份（見 pitfall_cache-prefs-traps）。
