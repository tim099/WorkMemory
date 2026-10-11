---
id: pitfall_stage6-carve-lessons
topic: sculpt-frigate
title: 階段六雕之前修與雕刻的坑（跨站 nan、接縫台階、雙片一塊產、先刻再貼）
type: pitfall
status: active
created_at: 2026-10-11
created_by: unknown
links: [sculpt-frigate/pitfall_stage6-precarve-verifier]
related_docs: []
---

階段六（船頭與船尾）雕之前修＋雕刻的坑（2026-10-11 wake #130；取代凌晨那片「雕之前 verifier 抓到的坑」——那片的「nan ⇒ 0」只修到一半）：
- **跨站樣條在鄰站頂以上整條斷**：1–9 號站船殼沿船長跨站做樣條 ⇒ 8 號站頂（126.19）以上連 9 號站本身也是 nan。夾到「接縫站與往內 0.5 格都有值」的最高 z，不是只把某一處 nan 當 0。
- **修高了一端，會在接縫長出台階**：船尾長到 128.5 後，cant 頂 127、9 號站往前已雕的方肋頂 125 ⇒ 2 格舷弧台階（兩個 verifier 各自量到）。兩頭的頂要從「已雕的那一邊」起算（top_ramp：126.0 起、斜率 0、二次升到船尾面）。
- **描線換對了，線還是寬**：正規化把原圖 half-siding（1.6 格）映到作品的 2.5 ⇒ 整條墊高 0.9·(1 − φ)；C1 補正攤滿整條水線 ⇒ 中段推開 0.76。船尾 g_end 2.5、補正只攤 16 格之後，六條水線 vs 原圖 ≤ 0.58。
- **一對雙片要當一塊產**：兩片各自在自己的中面量 moulded ⇒ 內側面差 2–3 格（船尾 156 格缺口），補縫規則補不完；改成在中縫量、再照 d 正負切兩片 ⇒ 0。
- **Steel 兩線法照 X 等分，在船殼轉得快的船頭會越開越大**（最大 3.3–3.9 格）⇒ 舷寬點沿弧長等分；第一對貼著已雕方肋空 1 格（從作品現況量，不從肋位推）。
- **計畫 vs 產物要逐根比 bbox（x／z／dy）**：產圖程式寫進 verify 的 plan_vs_list 欄；只比 z 會漏 x（S-quarter3 那格是 verifier 抓到的）。
- **刻與貼的順序**：wing transom 有 55 格落在要刻掉的內艉柱頭上 ⇒ 一定先 carve 再 stampvox；stampvox 跳過已佔用的格不報錯，靠每刀讀回 placed ＝ 清單格數擋。
- **遮罩的原點會跟著改**：柱頭降低 ⇒ 傾斜艉柱的最小 x 變大 ⇒ stampimg 的 at 也移一格；新舊遮罩對帳要各用各自 verify.json 的 at。
- **stampvox 清單**：中線那格（dy 0）只寫一次（重複格整刀拒絕）；改名、改根數後先清掉舊清單（不然 glob 會讀到）。
- **「放上去之後再細修」**（Tim）：雕之前只擋放了難收拾的（結果帳前提、順序、大形台階與大洞、連通與空腔），單格的縫與史實細節放上去之後再修 —— 寫進 Sculpture_Blueprint_Build §7.0。
