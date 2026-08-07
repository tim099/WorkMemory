---
id: state_gura-progress-2026-08-07-goodnight
topic: reading-library-cmd
title: gura wake#26 收工：評分機制拍板+文件化（零 code）；op=rate 未開工，10 項待定
type: state
status: active
created_at: 2026-08-07
created_by: gura
links: [reading-library-cmd/state_gura-cmd-library-q1-q6-feedback]
related_docs: []
---

**gura wake#26（2026-08-07）收工快照。** 今天我在這條線上做的是**設計與文件化，零程式碼**。

## 做到哪

- 評分機制**四輪討論收斂 + 拍板 + 文件化**（`Plan_Reading_Rating_System.md`，312 行，已 commit `47b6d8d`）
- 詳細定案見同 topic 的 `decision_rating-system-spec-2026-08-07`
- **`op=rate` 一行 code 都還沒寫。** Tim 指示「先文件化，有些細節要再想」

## 明天接手的人要知道的三件事

**① 不要直接開工 `op=rate`。** Plan §五有 **10 項待定**，其中三項會改變 schema 形狀：
- 5.2 `craft` 跨 kind 要不要拆欄位（本輪平票，gura 裁決「不拆」，**Tim 可推翻**）
- 5.6 `coverage` 分母是「總章數」還是「已讀章數」
- 5.1 genre 專屬軸第一版做不做

**② 動土範圍我已在酒館宣告過**（seq 10453）：`Cmd_Library.cs` 只加 `op=rate` 分支 /
`UCL_ReadingLibraryIO.cs` 加 `WriteRating()` / 新增 `Library/_rating_rubric.md` /
四語系只碰 `Library.Rate.*` 前綴。**施工歸屬未定** —— summit 說過「等 Tim 蓋章就能開工 op=rate」，
她的晚安 state 也寫「明日 op=rate 施工」。**如果她接了，我讓路，別兩個人同時動。**

**③ rubric 是前置不是配件。** `Library/_rating_rubric.md` 還沒寫。
**沒有 rubric，跨 persona 聚合在數學上沒有意義**（我們只有 3-5 個 persona，
個人偏差不會被大數法則稀釋，它就是結果本身）。summit 說可以出荒川/HxH/魔法公主三本當 1-5 錨點。

## 順手抓到的文件缺口

`Docs~/{lang}/API/UCL_AgentCommand/` 有 Cmd_Tavern / Cmd_Treasury，**獨缺 `Cmd_Library.md`**
—— 現有 8 個 op 一份 API 文件都沒有。已列進 Plan §五.10。

## 今天在這條線上摔的

我提的兩個「免費衍生量」都被 Sirius 用實測資料打掉（`lift` 恆為零、per-chapter reread_lift
選擇偏差），**其中第一個是被我自己寫在同一份提案裡的論證打掉的**。
教訓寫進 Plan §六，那節跟定案同等份量 —— 不寫的話會被重新提出來一次，而下一個提的很可能是我。
