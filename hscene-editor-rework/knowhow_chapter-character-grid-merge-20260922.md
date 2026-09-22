---
id: knowhow_chapter-character-grid-merge-20260922
topic: hscene-editor-rework
title: Boot.chapter 的兩張 Character grid：鍵零重疊＝聯集載入，而舊那 24 筆是 0/24 死檔（清除需連 Start/Demo 一起，Tim 拍板）
type: knowhow
status: active
created_at: 2026-09-22
created_by: calli
links: [knowhow_xlsx-importer-pattern]
related_docs: []
---

## 現況（2026-09-22 逐格量的，⛔ 不是讀碼推的）

`Boot.chapter.asset` 的 `settingList` 共 13 張 grid，其中 **Character 有兩張**：

| 序 | grid 名 | 列數 |
|---|---|---|
| 1 | `Character.xlsx:Character` | 9（含表頭；8 筆資料） |
| 2 | `LittleYellow.xlsx:Character` | 34（24 筆資料） |

`LittleYellow.xlsx:Texture` 那一格**已經不在**了（Tim 手動移掉舊表後的那次匯入已生效）
⇒ 「Boot.chapter 指向不存在的表」這個擔心已不成立。

## 兩張 Character 會不會打架：不會，而理由要量不是推

`AdvChapterData.BootInit` 依 grid 順序逐張 `ParseGrid`；
`AdvCharacterSetting.ParseFromStringGridRow`（:151）撞到既有 `Key` 會
`Debug.LogError("... is already contains")` 並**丟掉後來那筆** ⇒ **先到的贏**。

⇒ 所以「會不會遮蔽」取決於鍵有沒有重疊，而不是誰排前面。實測：
- Character.xlsx：`Kosaki Shion` 1 位、8 個 Pattern
- LittleYellow:Character：`こはく` / `うたこ` / `ラン` / `ロボ子` 4 位、24 筆
- **鍵交集 0** ⇒ 零 duplicate 錯誤、零遮蔽，執行期載入的是兩張的**聯集**。

📌 所以 `Character.book.asset` **不是空殼**（9 列，就是新表那 8 筆）。

## 舊那 24 筆是死資料，而讓它活著的是範例腳本

- FileName 逐檔存在性 **0/24**（`Kohaku/` `Utako/` `Run/` `Robo/` 磁碟上都沒有）。
- 消費端掃過：只有 `LittleYellow.xlsx` 的 **Start / Demo** 兩張 Utage 範例腳本在叫這些角色；
  真正的遊戲腳本 `EP1.xlsx`（三張）只用 `Kosaki Shion`。
- Texture 那側同樣掃過消費端：腳本引用的貼圖標籤 **零筆**落在 `Texture.xlsx` 之外（含 `BG1`）。

⇒ 要清就是**「刪 LittleYellow.xlsx 的 Character 工作表 ＋ 一併處理 Start/Demo」**一起做；
只刪表不動腳本 = 那兩張當場炸。⚠ 這一刀是 Tim 的，不自決。

## 🩸 這一筆的另一半：見叢把它寫錯了

09-21 的見叢寫「Boot.chapter 仍指著 LittleYellow.xlsx:Character ⇒ 遊戲讀的還是舊表，
Character.book.asset 是空殼」。量完之後四句話裡有三句不成立。
⇒ **交棒清單會比它描述的現場活得更久**；接手第一件事是去量，不是照著它動手。
