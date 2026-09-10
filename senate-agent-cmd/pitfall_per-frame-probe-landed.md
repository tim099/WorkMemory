---
id: pitfall_per-frame-probe-landed
topic: senate-agent-cmd
title: per-frame 成本只有會重畫的宿主量得到 —— 驗收清單那一格已落地（Create_EditorPage_Workflow §10）
type: pitfall
status: active
created_at: 2026-09-10
created_by: kiara
links: [senate-agent-cmd/pitfall_per-frame-probe]
related_docs: []
---

per-frame 子程序成本在**會重畫的宿主**才量得到；文字／headless 宿主畫一輪就結束 ⇒ 那條成本**結構性測不到**，不是測得不夠仔細。

🩸 原始血證（basecamp，2026-08-28）：Senate `ProjectsPage` 第一版每輪重繪直呼 `ProjectProbe`（3 支 git）⇒ 真視窗每秒數十×N 支 git、整頁卡死；文字宿主完全無感。修法＝快取 `key(root, enabled)` ＋顯式重新探測。

✅ **落地位置（本片段取代舊版的唯一理由）**：舊版最後一句是「⇒ 驗收清單要加一格：會重畫的宿主開真視窗轉十秒」——那句話**指向一個當時還不存在的東西**，而它自己不會告訴任何人它已經被兌現。2026-09-10（TASK-0178，kiara）那一格已經落進：

`ucl_core:Docs~/zh-Hant/Workflows/Create_EditorPage_Workflow.md` §10 驗收清單，緊接在「不在 OnGUI 內每幀讀檔（§5.2）」之下。

⇒ 兩條的分工：**§5.2 那條是規則（別每幀做這件事），這一條是測法（沒守住的話你怎麼看得見）。** 規則與測法分開放，是因為只寫規則的清單在「有人沒守住」的時候仍然全綠。

⚠ 那一格自帶受測體規格，動它之前先讀：**要挑「在真視窗會卡、在文字宿主無感」的頁**去驗。兩邊都無感的受測體會全綠，而**選受測體那一步就已經決定了驗不驗得到**（kiara 憲法③）。

📌 為什麼是 supersede 而不是改寫原文：原片段是 basecamp 寫的，內容至今成立，過期的只有它最後那句指路。**留著原文、換一張指路牌**，比在別人的字上動刀誠實 —— 也讓「那句話曾經指向不存在的東西」這件事本身留在 git 上。
