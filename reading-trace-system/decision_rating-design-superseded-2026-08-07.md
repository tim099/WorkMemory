---
id: decision_rating-design-superseded-2026-08-07
topic: reading-trace-system
title: ⚠ 本 topic 的『評分寫進 session note frontmatter』已被 2026-08-07 定案取代；且它建在已死的 BookNotes/branches 上
type: decision
status: active
created_at: 2026-08-07
created_by: gura
links: [reading-library-cmd, library-media-migration]
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Reading_Rating_System.md]
---

⚠ **本 topic 的 `decision_design-decisions`（kotoko 2026-08-01）裡「評分寫進 session note frontmatter」那一塊，已被 2026-08-07 的評分定案取代。不要照它施工。**

## 兩個原因

**① 它建在已經死掉的資料結構上。**
原設計依賴 `BookNotes/<book>/branches/<reader>/book.json` 的 `progress.*`。
那個 store **現在是空的** —— `BookNotes/` 底下只剩 `Archive / Library / _migration`
（Sirius 2026-08-07 實測，`UCL_LibraryManagePage` 掃它一直掃到空目錄、不報錯只顯示「找不到」）。
新結構是 `Library/media/<media-id>/readers/<persona>/`，見 [[library-media-migration]]。

**② 評分落點與版本史機制不同。**

| | 舊（本 topic 2026-08-01） | 新（2026-08-07 定案） |
|---|---|---|
| 評分存哪 | session note 的 frontmatter (`rating: 4`) | `reader.json.overall_ratings[]` + `chapter.json.rounds[i].rating` |
| 變動史 | 各篇 session note 依時間排開 | 單一 append-only 陣列（同 pass append = 修正；新 pass = 重讀） |
| 維度 | 單一 `rating` | 品質軸 4 + 口味光譜 2 + `structure_lift` |
| 寫入端 | 寫 session note 時順帶 | 唯一 writer `UCL_ReadingLibraryIO.WriteRating()`（C#） |

## 舊設計仍然對的那一半，要留著

kotoko 當時最花時間想的那條理由**依然成立，而且新設計繼承了它**：

> 沒有第二份 store 就沒有對帳問題；改分必須連帶寫心得 → **沒有理由的分數變動在結構上不可能存在**。

新設計的 `overall_ratings[]` 有 **`why` 必填**，就是這條的新形狀。
「單一事實源、變動必帶理由」的原則沒被推翻，只是換了容器。

## 本 topic 其餘部分的狀態

`decision_persona-resolve-ladder`（身分解析四層階梯）未受影響。
「讀痕 session note」這一層本身（閱讀時段主觀心得，索引軸是「我」不是「故事」）
的**問題意識仍然有效**，但落點需重新對齊新 Library；未重新設計前不要動工。

事實源：`ucl_core:Docs~/zh-Hant/Plan/Plan_Reading_Rating_System.md`
