---
id: pitfall_fork-pipeline
topic: sculpt-gloire
title: 抄別人的造船管線會咬人的七格
type: pitfall
status: active
created_at: 2026-10-16
created_by: kaguya
links: []
related_docs: []
---

抄別人的造船管線（光榮號抄拿破崙號）會咬人的幾格 —— 2026-10-15 kaguya 實際撞到的：

1. **寫死的高度與層名**：draft_lines 的讀數高度寫死 113／150（原船的上砲甲板與舷頂）⇒ 換船 IndexError；build_decks／check_decks 寫死四層甲板；draft_fittings 把零件寫死在 `甲板-spar`。換船先 `grep "'upper'\|'spar'\|113\|150"`。
2. **「子作品已存在」的路徑判斷多一層 dirname** ⇒ 永遠判不存在。新版工具寫進去之前舊版先跑了，多建一件空的子作品（沒有刪作品的 op，只能改標題標「不使用」）。⇒ 改工具用 Write 要先 Read，**改完再跑**。
3. **關聯 meta 要讀回**：落格工具建關聯時只寫了 note、漏了 meta（material=iron）——是 Tim 一句「鐵甲記得標材質」先擋下來的。之後每個帶 meta 的關聯都 `work sub=meta` 再寫一次＋`work sub=links` 讀回。
4. **舷側往內收（tumblehome）的船，錨貼船殼時錨環會懸在外面** ⇒ draft_anchors 2,100 個候選全被「錨環離實心 > 5 格」淘汰。解法是照史料加 cathead（從艏樓舷牆伸出舷外的橫木），錨環掛在它底下。
5. **遊戲 geo 沒填 ⇒ 沒有 carronade 的船，軍官／舵手站不下**：預設露天甲板站人高度 z 100–109（憲法號）。光榮號上甲板 z 114、艉樓 139 ⇒ geo.spar = [113, 141] 才 ✗0 ⚠0。交船時把這個數字寫給遊戲那側。
6. **彈藥庫天花板跨過 orlop 的舷弧台階** ⇒ 夾出一層封閉空腔（1,936 格，draft_magazines 的 seal 擋下）。彈藥庫的 x 範圍放在同一個舷弧平段裡。
7. bash heredoc 裡的 `\n` 會變真換行（又一次）⇒ 有跳脫字元的 patch 一律寫成檔或用 Edit。
