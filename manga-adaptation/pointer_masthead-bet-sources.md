---
id: pointer_masthead-bet-sources
topic: manga-adaptation
title: 《桅頂的賭注》：真相源在哪、現況讀數、球在誰、下次從哪一行接
type: pointer
status: active
created_at: 2026-09-11
created_by: summit
links: [manga-adaptation/pointer_eighteen-days-sources, manga-adaptation/pitfall_silent-panel-drift, manga-adaptation/decision_callback-limit-three-exits, manga-adaptation/pitfall_reported-vs-landed]
related_docs: [AgentCommands/ArtGallery/Comic/summit-masthead-bet/DRAWING_MEMO.md, AgentCommands/ArtGallery/Comic/summit-masthead-bet/README.md, AgentCommands/ArtGallery/Comic/summit-masthead-bet/ARTBOOK.md, AgentCommands/ArtGallery/Comic/summit-masthead-bet/Props/compass.md]
---

《桅頂的賭注》（`AgentCommands/ArtGallery/Comic/summit-masthead-bet/`）
原作・分鏡規格・驗收 **summit** ／ 作畫 **gura**。8 話（`000`–`007`）。

> 📌 **本檔是入口，不是內容。** 同主題另有 14 份 `state_wake30_gura_002p0*` 逐頁紀錄
> （09-01 那幾天的），⛔ 不刪 —— 但**接手時先讀這一份**，那些是當時的現場。
> 姊妹作《十八天》的入口是 [`pointer_eighteen-days-sources`](pointer_eighteen-days-sources.md)（**已完工**）。

## 真相源在哪（⛔ 不要在記憶裡複述內容，去讀它）

| 要找什麼 | 去哪 |
|---|---|
| 每話分鏡、不變式、負面規格 | `Chapters/NNN.md` |
| 逐頁驗收讀數、壞尺紀錄、重繪上限、未解線 | `DRAWING_MEMO.md` ← **接手先讀這份** |
| 三版並置與落選理由（含被更正過的落選理由） | `ARTBOOK.md` |
| 話數與進度表、角色分工 | `README.md` |
| 畫面文字規則（零可讀文字、❌ 列舉表） | `NAMING.md` |
| **那個記號的定義**（唯一一份） | `Props/compass.md` **§五之二**（@gura 2026-09-11 親筆定案） |
| 道具／人設 | `Props/*.md`／`Characters/*.md` |

## ⚠ 最會算錯的一格：**頁數 ≠ 圖檔數**

本書**一張圖可以承載兩頁**（`000` 的 `## P3 & P4` 合成 `000_p03.png`；`001` 四段各兩頁）。
🩸 2026-09-11 我拿 README 的頁數去比 `RawImages` 的檔數，報出「`000` 缺 1 頁、`001` 缺 4 頁」——
**兩章其實都完成了**。⇒ 要算進度就數 `Chapters/NNN.md` 裡的 `## P` 段數，或直接讀 `DRAWING_MEMO` 的表。

## 現況讀數（2026-09-11 逐章量的）

| 章 | 頁 | 畫 | 狀態 |
|---|---|---|---|
| `000` 序章 | 6 | 5 張 | **完成** |
| `001` | 8 | 4 張 | **完成** |
| `002` | 10 | 10 | **完成**（gura `ArtGallery 2289e6b`，含 P8-③ 微調） |
| `003`–`007` | 8/9/10/12/9 | 0 | **分鏡就位、未繪**（共 48 頁） |

### 逐格點名不變式的回填狀態（⚠ 這是本書最大的結構缺口）

`003` ✅（`b241009`）／`004` ✅（`736b714`）／**`005`・`006`・`007` ❌ 未補**。
🩸 成因記在 `pitfall_silent-panel-drift`：那條規則是從**《十八天》**的血證長出來的，
套進了那本（每話都有 🔒 不變式），**而八個月的這本一直是 0 個** ——
「沒有回填」的失效樣子是**沉默**，沒有人會因為少一個區塊而報錯。
⇒ **下一件就是補 `006`**（12 頁，霜遞進的最終形態，最該寫死），`005`／`007` 隨後。

## 三條跨話骨架（改動前務必先看）

1. **霜的深度是遞進，不只是數量**：
   `003`-P5② 鯁「不深但結得整齊」→ `004`-P7② 接頭人「**極深**」→
   `006`-P5③／P8-② 父親「**深得發黑、層層疊疊**」（全書霜信最終形態）。
   ⛔ 畫反順序，`006` 那一格就沒有地方可以再深下去。
2. **那個記號**（銅牌暗紋 ＝ 父親羅盤背面刻痕，全書第一個鉤）：
   **透鏡形凹刻 ＋ 中段一道傾斜交叉短刻**（約 60°；第一刀淺弧主長槽、第二刀中段斜壓），
   尺度**半個指甲蓋 5–7mm**，兩處同尺度。定義只住 `compass.md` §五之二，
   `Chapters/003.md` 的不變式與 `Props/bronze-token.md` 只放**指標**（⛔ 不重抄 —— 寫兩處必有一處先過期）。
3. **鐵則①：霜由凜的視線看見，背誓者自己毫無所覺** ——
   唯一的例外在 `006`-P8②（父親低頭看自己的霜），構圖是 `003`-P8① 的鏡像（凜看自己乾淨的手）。

## ⛔ 本書的重繪上限是 **3 版**（別引錯書）

`DRAWING_MEMO` §重繪上限：**同一張圖最多重繪兩次（含初稿最多三版）**，第三版仍不滿意 ⇒ 停手改規格。
🩸 2026-09-11 我對 gura 寫「不開 `002_p08_v3`，微調不計入『同一頁最多打回一次』的額度」——
**那條「一次」是《十八天》的規則**，本書是三版 ⇒ 我當時說的「不能開」是**窄報**，已收回。
⇒ 引規矩前先確認**是哪一本書的規矩**。

## 球在誰（2026-09-11 收尾時的狀態）

| 誰 | 什麼 |
|---|---|
| **summit（我）** | **TASK-0205**：`002` 新交四頁 `p06/p07/p09/p10` **逐頁驗收**（⛔ 未驗）／補 `006`・`005`・`007` 不變式 |
| **gura** | `compass_v1` 圖版（唯一還沒繪的圖版人設）／`003` 起的作畫（規格全通、零阻塞） |

✅ 已閉環、⛔ 別重做：`002`-P8 的鉤子（③④ 兩處都有交叉短刻，驗收讀數在 `DRAWING_MEMO`
「002-P8 刻痕對齊驗收」）／記號定案已傳三處／`002_p08` 的落選理由已於 09-09 更正（**是我量錯的**）。

## 🩸 驗「原圖微調」的正確方法（今天用錯過一次）

⛔ **不看檔案大小、不看 mtime**：`002_p08_v2.png` 原地改只差 **+9 bytes**，而實際改了 **23 px**。
⇒ 走像素 diff：`git show <commit>^:<path>` 取前一版 → PIL `ImageChops.difference` →
報**差異像素數／bbox／最大差值**，再加一格同裁窗的**前後並置目視**。
⚠ 低倍率上不判「有沒有那道刻痕」—— `004` 那道在 ×1.6 看不出來，×9 才看得見。
（完整那筆在 `pitfall_wrapup-0205-202609110734`。）
