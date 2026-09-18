---
id: pitfall_orphan-bookshelf-card
topic: reading-library-cmd
title: 孤兒閱讀卡：投影有機械戳而真相源不存在 —— §6.6 的 CardOrigin 放它過去
type: pitfall
status: active
created_at: 2026-09-18
created_by: kaguya
links: []
related_docs: [repo:AgentCommands/Tasks/tasks/0246.md]
---

本筆是這條線的**消費端**那半（既有 33 筆多在寫入端 `Cmd_Library`／服務層）。TASK-0246。

## 坑的形狀：孤兒卡通過了唯一那道檢查

`letters/<persona>/bookshelf/*.md` 是 `reader.json` 的機械投影。而 §6.6 見書
（`SCP_Core/Runtime/Letters/SCP_WakeBrief.cs` 的 `BookshelfSection`）**只讀那個目錄、不碰 `BookNotes/Library`**
（明文射程，與 §6.5 讀 sketchbook 同一條界線）⇒ 它**結構上**看不到卡片的真相源在不在。

⚠ 而那一段**已經有**一層定語：`CardOrigin()` 判 Mechanical／Legacy／Unreadable，計數行印
「其中 N 張不是機械投影／N 張檔頭讀不到」。
🩸 **孤兒卡有機械戳** ⇒ 它是「合格的機械投影」，那道檢查放它過去。
⇒ `CardOrigin` 回答的是「它是不是寫入端產的」，沒有人問「它指向的那筆 `reader.json` 還在不在」。
**同一個量的第三種相：解出來了，而它指向的東西不在。**

## 讀數（2026-09-18，可複驗）

122 張卡 / 114 有真相源 / **4 孤兒** / 4 缺 `media_id` 讀不到檔頭。
孤兒那 4 張**全在同一個 media**（`stream-bilibili-xiaozhong-johnny`：calli／kaguya／kiara／meadow）
⇒ 單一事件（那部的四個 reader root 一起不在），不是四個獨立意外。
⚠ 缺 `media_id` 那 4 張是**另一種形狀**（`Template/test-fixture-book.md` ＋ summit 三張舊 schema 命名），修法不同。

## 拍板（Tim 2026-09-18 授權）

走 **(a) 防讀**（加第四態 `Orphan`，**不過濾抽籤母體、只標定語**）；
⛔ **不走 (b) 反向清理投影** —— 那 4 張的正文是那幾份心得的**唯一副本**，而且是摘要級的。
⭐ 「不過濾、只標定語」這個判準**不是新發明**：該段註解自己寫著「過濾掉會讓『這張卡是舊時代的』與
『它不存在』同形…所以照抽，改成標定語」。⇒ 延伸既有判準到第四態，不是另立一套。

## ⛔ 三個下次會重踩的坑

1. **改哪一份 SCP_Core**：要改 `D:/Unity/Senate/SCP_Core`（實測比 LY/Assets 那份**新 4 個 commit**）。
   在 LY 那份改＝製造分岔。而那側要**排施工場的隊**（本次被 @summit 的 `SCP_Core\Runtime` 擋下，租期 13:17）。
2. **修症狀之前先留受測體**：本單 ⑧（回填 kaguya 那張）把 ⑦（迴歸讀數）的受測體修掉了 ——
   修完本 persona 剩 0 張孤兒，⇒「§6.6 沒印警語」會被當成通過。
   ⇒ 迴歸要**人工造**一張（`Template` persona 是為此存在的）。⛔ 別拿別人的 persona 當受測體（那是替她生成 brief）。
3. **搜酒館訊息的射程**：訊息是**一則一檔** `rooms/<room>/messages/<date>/000NNNNN.json`，
   ⛔ **不是** `messages.jsonl`。🩸 首版搜法找 `*.jsonl` ⇒ 回「0 筆」，而**連已知存在的 seq 19198 都找不到**。
   ⇒ 抓到它的是**對照組**（拿一則已知存在的去搜），不是更仔細。
   📌 少那格對照組，「沒有第二個副本」與「搜法壞了」逐位元組同形 —— 而本單正是靠那個「0 筆」
   判定「清理投影會銷毀唯一副本」，⇒ 那個拍板的地基就是這格對照組。
