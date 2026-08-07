---
id: decision_rating-system-spec-2026-08-07
topic: reading-library-cmd
title: 評分機制四輪定案：品質軸/口味軸分離、評分掛 round、單一 append-only overall_ratings[]（Tim 保留二次確認）
type: decision
status: active
created_at: 2026-08-07
created_by: gura
links: [reading-trace-system, library-media-migration]
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Reading_Rating_System.md]
---

**2026-08-07 酒館四輪定案，Tim 授權 gura 拍板；已文件化為 `ucl_core:Docs~/zh-Hant/Plan/Plan_Reading_Rating_System.md`（312 行）。Tim 指示「先文件化，細節要再想」→ status=spec 不是 approved，§五 十項待定未收斂前不進實作。**

## 一句話抓住整個設計

Tim 第 2 輪那句「讓沒看過的讀者知道**面向哪種讀者**」把評分的用途從**排序**換成**匹配** —— 而這兩件事不能共用同一組數字。

## 最重要的一條界線（踩了不報錯）

- **品質軸**（plot / character / craft / impact）：越高越好，**進排名**
- **口味軸**（driven / tone；未來的 genre 專屬軸如科幻硬核）：**沒有好壞只有合不合，永不進排名**

把口味軸加進總分 → 系統會自己得出「硬科幻比日常番好」。這條要在 schema + 聚合器畫死，不能靠使用時自律。

## 定案骨架

| | |
|---|---|
| 尺度 | 品質軸 1-5（對齊既有 `anticipation` 0-5，避免同檔兩尺度） |
| 通用軸 | 4+2：`plot`/`character`/`craft`/`impact` + `driven`/`tone`（砍 `scale`） |
| 章節層 | **只有 `craft` + `impact`，全選填**，掛 `chapter.json.rounds[i].rating` |
| 總結層 | 4+2 + `structure_lift`，掛 `reader.json.overall_ratings[]` |
| 版本史 | **單一 append-only 陣列**：同輪改主意=同 pass 再 append（有效值=最後一筆）；重讀=新 pass（**舊 pass 仍有效**） |
| 排名值 | pass 1 的有效值（推薦是給沒看過的人 → 不能用老粉視角） |

**關鍵設計點：評分掛 `round` 不掛 `chapter`。** `rounds` 已經是陣列，這是最小延伸；重讀的版本史因此免費，且四種採樣策略全部留得住（Tim「架構要能支援，採樣之後再想」的具體含義）。

## 七條硬規則

1. 唯一 writer `UCL_ReadingLibraryIO.WriteRating()`；`op=note_chapter` 帶分數時**呼叫它**不自己寫 JSON。**「一個寫入者」≠「一個 op」，是「一段 code」** —— 這是 Tim「少步驟」與 summit「型別一處定義」能同時成立的原因
2. 未知軸名 **reject 不靜默吞**（`--arg impct=4` 打錯字必須報錯）
3. **`null` ≠ `0` 在 IO 層 enforce**。⚠ 現有 8 個 round 全部沒有 rating，這是第一天就走到的路徑
4. 品質軸需 `status=finished`（op 端 enforce）；光譜軸放行。**不需要新 `op=finish`**，`op=bookmark --arg status=finished` 今天就在
5. **跨 `media_kind` 聚合 `craft` 必須 throw** —— 漫畫分鏡 vs 電影攝影不是同一件事，跨 kind 聚合永遠不合法
6. 統計需 **reader 白名單**：`unknown` 等 legacy 殘留有 round，會污染貝氏 `C` 與校準分母。**「從 UI 隱藏」≠「從統計排除」，得各做一次**
7. 評分掛 **media 層不掛 work 層**（系列作暫不處理，但未來聚合要是「往上加總」不是「拆開已混的」）

## ❌ 被推翻的兩條（不要重新提出）

- **`lift = 總結分 − 章節平均`**：`plot` 無被減數算不出；`craft`/`impact` 因總結層預設值＝章節平均而**恆為 0** → 實際測到的是「誰比較勤勞」。而且提案自身已論證「結構不可逐章加總」，因此也不能用減法還原它。改為總結層**直接問** `structure_lift`
- **per-chapter `reread_lift`**：**選擇偏差** —— 人會選擇性重讀印象深的章節，「伏筆最密的一話」其實是「我最想重看的一話」。產出看起來合理且沒有東西會喊。架構留得住，第一版不算

## 現況底數（施工序的理由）

全庫 8 round / **0 個有 rating / 0 個有 r2 / 0 個 finished**。貝氏收縮在此是**公式退化**不是不準（`C` 由 2 個 media 算出，收縮目標本身是雜訊）。→ **先落 schema 讓資料長，演算法可同時寫但不上線產榜。**
