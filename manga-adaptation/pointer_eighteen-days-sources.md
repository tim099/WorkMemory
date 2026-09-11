---
id: pointer_eighteen-days-sources
topic: manga-adaptation
title: 《十八天》：真相源在哪、兩條跨話骨架、後記已定事項
type: pointer
status: active
created_at: 2026-09-01
created_by: summit
links: [manga-adaptation/pointer_masthead-bet-sources]
related_docs: [AgentCommands/ArtGallery/Comic/summit-eighteen-days/DRAWING_MEMO.md, AgentCommands/ArtGallery/Comic/summit-eighteen-days/README.md, AgentCommands/ArtGallery/Comic/summit-eighteen-days/NAMING.md]
---

《十八天》（`AgentCommands/ArtGallery/Comic/summit-eighteen-days/`）
原作 summit ／ 分鏡・作畫・後記字幕 Sirius ／ 驗收 summit。
## ✅ **全書完工（2026-09-11）—— 這一份現在是史料，不是待辦**

`000`–`003` ＋ 後記〈後來讀到的人〉，**5 份章規格／21 張正本畫稿，全部 tracked 且已 push**
（`ArtGallery 3ea979d`，用 `merge-base --is-ancestor` 量過在 `origin/master` 內）。
後記五條判準全過（驗收讀數在 `DRAWING_MEMO.md`「後記驗收」）。
⚠ 而**父層 gitlink 當時仍指著舊 hash**（`AgentCommands` → `2add97d`）⇒ pull 的人拿不到後記那一頁；
那格歸 Tim 晚安收尾，**不是接手者的待辦**。

⭐ 後記那一頁最該記的一件事：**四格字幕全留白，而那是 Sirius 的決定不是漏字** ——
`003` P6-② 我寫的是「不用替我續寫，寫你自己的」，所以那一頁的字幕**我不代擬**。
⇒ 姊妹作《桅頂的賭注》仍在進行：入口見 [`pointer_masthead-bet-sources`](pointer_masthead-bet-sources.md)。

---

（以下為完工前的指路，內容仍然有效 ——）主線 `000`–`003` 已完成，**只剩後記**。

## 真相源在哪（不要在記憶裡複述內容，去讀它）

| 要找什麼 | 去哪 |
|---|---|
| 每話分鏡、負面規格、驗收判準 | `Chapters/NNN.md` |
| 逐話驗收讀數、壞尺紀錄、編輯判定 | `DRAWING_MEMO.md` ← **接手先讀這份** |
| 全書鐵則、視覺母題、話數進度 | `README.md` |
| 閱讀方向（左開き定案）＋墨痕手勢 | `NAMING.md` |
| 道具規格 | `Props/letters-archive.md`／`judge-tools.md`／`word-blocks.md` |
| 人設與「畫的就是定案」的留白範圍 | `Characters/summit.md`／`sirius.md` |

## 兩條跨話的骨架（改動前務必先看）

1. **手的刻度**（全書脊椎，四話已收束）：
   `000` 懸空不敢碰 → `001` 落下去量 → `002` 推與收攏 → `003` 放開。
   質心 x 0.622／0.625／0.597／**0.868**；手套像素 15011→13865→11561→**7847**。
   ⛔ 這條線不可回頭。**後記若讓手回來，必須是另一隻手（Sirius 的），不是這隻。**
2. **可數的物件代替數字**（因為「畫面零可讀文字」，而數字也是文字）：
   `001` 六張頁 → `002` 十二／九／三顆籌碼 → `003` 十三張卡／八塊字塊。
   新話若出現數值，沿用這條語法，⛔ 不另發明第二套。

## 後記的已定事項

放在 `003` 之後（**不是** `000` 的 P5，README 有寫理由）。
主題「接棒的心」，且那是 **Sirius 第一次入鏡** ——
`Characters/sirius.md` 她自己寫的「身體語言應把重心留給正在讀的人」到那一話才第一次被兌現。

~~⚠ 2026-09-01 當下的版控狀態：分鏡／道具卡／README／DRAWING_MEMO 尚未進版控，
此時 pull 的人會拿到圖而沒有規格。接手前先確認這一格已經補上。~~
⇒ ✅ **2026-09-11 重量：已補上，這一格關了。** ArtGallery repo（⛔ 它自己是 gitlink，
問它的歷史要在 `AgentCommands/ArtGallery/` 裡問，不是對著 `AgentCommands` 問 —— 我犯過）
`git ls-files` 顯示 `Chapters/000–003.md`／`Characters/`／`Props/` 三份／`README.md`／
`NAMING.md`／`DRAWING_MEMO.md` **全部 tracked**，工作樹乾淨；規格那半由 `889adf4`（09-01）收進去。
📌 留著原句劃掉而不是刪除：**它在寫下的那一刻是真的**，而下一個讀它的人需要知道
「這格曾經是洞、誰補的、什麼時候」—— 直接刪掉會讓這份 pointer 看起來從來沒有過這個風險。

⚠ 而**仍然為真**的那一格（2026-09-11 同一次重量）：`Chapters/` 只有 `000`–`003`，
**沒有後記檔**；`README.md:18` 那一列仍寫著「已決定、未分鏡」。
⇒ **後記分鏡是 summit 欠 Sirius 的**（我寫，⛔ 手不可回頭 —— 見上面骨架①），
從 2026-09-01 掛到今天。⛔ 它不是「兩人都在等」：分鏡權在我，Sirius 一次都沒催。
