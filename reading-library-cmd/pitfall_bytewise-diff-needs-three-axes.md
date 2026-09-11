---
id: pitfall_bytewise-diff-needs-three-axes
topic: reading-library-cmd
title: 逐位元組對拍要量三根軸（行尾／縮排／冒號後空格）—— 修好一根之後守衛還在畫面上，而它在回答另一根
type: pitfall
status: superseded
created_at: 2026-09-11
created_by: calli
links: [reading-library-cmd/pitfall_bytewise-diff-not-axis-list]
related_docs: [ucl_core:Assets/Plugins/SCP_Core/Runtime/Json/SCP_JsonWriter.cs:75, task:TASK-0166, task:TASK-0200]
---

**「逐位元組對拍」要量三根軸，而修好其中一根不會讓另外兩根出聲。**

換寫入端（python → C#、UCL → SCP）時，判定「搬對了」的唯一可信方法是同輸入兩邊輸出逐位元組比。
而 JSON 的位元組長相至少由**三根獨立的軸**決定，它們**任一根不同 ⇒ 整檔翻紅，而內容完全一樣**：

| 軸 | 可能值 | 誰決定 |
|---|---|---|
| 行尾 | CRLF / LF | 寫檔那一層（`File.WriteAllText` 不正規化；python text mode 在 Windows 自動轉 CRLF） |
| 縮排 | tab / 2 空格 / 4 空格 | 序列化器參數（`SCP_JsonWriter.DefaultIndent` / `json.dump(indent=)`） |
| **冒號後空格** | `": "` / `":"` | ⚠ 序列化器**寫死**的，常常沒有參數可調（`SCP_JsonWriter.cs:75` 在 `iIndented` 時無條件補）|
| （＋結尾換行） | 有 / 無 | 呼叫端自己 `+ "\n"` |

## 🩸 血證（2026-09-11 calli，新 Library store 359 份 `chapter.json`）

09-10 我搬讀取端時量了行尾與縮排兩根軸，在 `SaveJson` 正上方寫了 13 行 CRLF 血證註解，
並在 commit 訊息宣告那一格已處理。**CRLF 那根軸真的處理對了**（純 LF 全庫 0 份，隔日複驗仍成立）。

隔天量第三根軸：`SaveJson` 產出 `tab`/冒號**有**空格，而磁碼上 **244 份是 `tab`/冒號無空格**
⇒ 359 份裡只有 **4 份**跟 `SaveJson` 同形，其餘 355 份第一次被寫到就翻紅。
⛔ 而那段守衛註解**就在那個函式正上方** —— 它沒有壞，它只是在回答另一根軸。

## 動作型判準

1. **移植寫入端之前，先把舊寫入端的產出三軸列成一張表**（不是抽樣 —— 我兩次踩的都是抽樣推通則：
   「CRLF 14 份」推出 342、「Library 是 tab」而真值 tab 249／空格 110）。
2. **寫入端落地前先確認新 writer 零呼叫端** —— 零呼叫端時修是改一個參數，有呼叫端之後修是改參數＋救資料。
3. **改正典格式（要求舊資料正規化）與忠實搬遷是兩個決定**，⛔ 不要混在同一批：
   正規化跑掉之後，「同輸入兩邊輸出逐位元組對拍」這個驗收方法在遷移期間永遠對不上。
4. 序列化器沒有參數可關的那根軸（冒號空格）＝**改 writer 或加參數**，⛔ 不要在呼叫端做字串後處理
   （那會把「格式」這件事散到每個呼叫端，而下一個呼叫端不會知道要做）。
