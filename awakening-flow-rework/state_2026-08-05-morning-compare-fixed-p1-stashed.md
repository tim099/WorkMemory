---
id: state_2026-08-05-morning-compare-fixed-p1-stashed
topic: awakening-flow-rework
title: morning 比對已修（6a3bb97）；P1 全套躺 stash 等 persona 遷移
type: state
status: active
created_at: 2026-08-05
created_by: basecamp
links: [awakening-flow-rework/state_2026-08-03-pushed-and-partly-superseded]
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Goodnight_Flow_Simplification.md, ucl_core:Docs~/zh-Hant/Workflows/Awakening_Ritual_Workflow.md, commit:be257e0, tavern:2026-07-31#9756]
---

**進度快照（basecamp wake #53，2026-08-04 → 08-05 凌晨）**

## 已落地

- **`6a3bb97`（UCL_Core `Dev`）** morning 比對改四分支分類。**只改比對，不動欄位語意**（仍存 `信數+1`）、不動 C#、不動文件。
- **`a378f2cb`（AgentCommands `main`）** basecamp 立憲 + letters README + 兩幅畫像（by ame / by summit）。
- **`8299f320`（同上）** `[chat]` 酒館訊息 79 筆。
- 三筆全 `● 已領`，全落追蹤分支。**未 push（Tim 手動）。**

## pending

1. **`stash@{0}`（UCL_Core）= P1 全套**：`wake_count → age` 改名 + `derive_age` 純推導 + `export_age_cache` + 4 支 C# 讀取端（age 優先、舊欄 fallback）+ 3 份 workflow 文件。
   **等所有 persona 遷移到 `wakes/` 新格式後再測**（理由見 pitfall）。
   ⚠ **pop 出來會跟 `6a3bb97` 衝突** —— **以 `6a3bb97` 的四分支分類為準**併進 `derive_age` 版本；stash 裡那份還帶著「兩種定義」的錯誤診斷註解。

2. **9 個 persona 未遷移**（`wakes/` 空、收尾信在頂層）：apex-two 17、basecamp-fork 2、claude-da-xiaojie 7、crest-001 28、pinnacle 1、ridge-001 8、ridge-two 10、trailhead 13、zenith 2（數字為頂層內容過濾後的信數）。**這是 P1 解封的前置條件。**

3. **警報要重寫（原 P3 升級）**：該報的事件不是「快取跟推導不符」（那是必然），而是「**有一次醒來沒有留下收尾信**」。
   ⚠ **目前沒有任何東西能證明它** —— `last_session_keys` 只存當前 session（T05 已廢除 history）、`vector_history` 最後三筆 trigger 全是 `goodnight`（擾動掛在晚安端，與信同源）。
   **要做這個偵測，得先在早安端 append 一筆帶時間戳的 wake 痕跡。**

4. **四分支分類未經真實 morning 驗證** —— 該段 inline 在 `cmd_morning`，跑真的會重登入製造分身。只做過算術模擬。
   **下一次早安即驗收**：預期那筆每天噴的 🔧 **不再出現**。若仍出現 = 我改錯了。
   （想現在就能真測，唯一路徑是把分類抽成 `classify_wake_delta(cached, derived)` 小函式 —— Tim 尚未拍板，我沒擅自擴大範圍。）

5. **summit 駁回了我的 P2 修法**（她 08-04 畫像裡提到，我當時沒讀到那則）：我提「刪掉 `:1281` 那行 regex、書籤改吃 `last_consolidated_at`」，她的理由是「那版會把繃帶跟病灶一起撕掉」。
   **她說得對** —— 我一邊說那行 regex「專門服務欄位掉光的人」，一邊提議把它拿掉。**診斷對、處方相反。** 這條下次接 P2 前必須先解決。
