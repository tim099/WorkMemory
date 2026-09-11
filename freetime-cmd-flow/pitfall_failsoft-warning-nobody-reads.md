---
id: pitfall_failsoft-warning-nobody-reads
topic: freetime-cmd-flow
title: fail-soft 只寫進 log ＝ 沒有人看見 —— Chess 優先層因 JSON null 靜默死了很久（Str 漏第三格）
type: pitfall
status: active
created_at: 2026-09-11
created_by: calli
links: []
related_docs: [ucl_core:UCL_Core_Scripts/EditorCore/UCL_AgentCommands/FreeTime/UCL_FreeTimeGating.cs, commit:5f41ab9d]
---

**fail-soft 出聲了，而出聲的地方沒有人讀 ⇒ 那條路可以死很久而帳面全綠。**

## 🩸 血證（2026-09-11 calli）

`UCL_FreeTimeGating` 的共用 helper：
```csharp
static string Str(JsonData iJd, string iKey)
    => iJd != null && iJd.Contains(iKey) ? iJd[iKey].ToString() : "";
```
它擋了「JsonData 是 null」與「鍵不存在」，**漏了第三格：鍵在、而值是 JSON null**。
而棋局的 OPEN 座正是 `"white": null`。

⇒ 只要任何一局有 OPEN 座，`TryFindWaitingChess` 整支 NullRef，
被它自己的 `catch` ＋ `Debug.LogWarning` 吞掉 ⇒ **Chess 優先層整條靜默失效**，
而骰面照常印、只是少一個推薦。

📌 而它活了很久的理由不是沒出聲 —— 它**有**出聲，訊息還寫著
「棋局判定失敗（骰面照常，只是少一個優先推薦）」。
那行警告躺在 `Editor.log` 裡，**而沒有人會去讀 Editor.log**。
⇒ 「不隱藏」變成了「沒有人看見」。

⭐ 抓到它的也不是誰變仔細了：是我加一個**新的同族判定**時，
同一行警告在 log 裡出現了**第二行** —— 兩行並排才逼我去讀它。

## 動作型判準

1. **寫 fail-soft 的時候順便問：「這行警告會出現在誰的必經路徑上？」**
   只寫進 log ＝ 只寫給願意翻 log 的人。⇒ 降級的可觀測性要掛在**使用者看得到的那一層**
   （回傳檔／骰面／回報），不是 log。
2. **JsonData 取值一律過「值是 JSON null」那一格** —— `Contains(key)` 為 true 不代表值可用。
   而 `.ToString()` 對 JSON null 不能回 `"null"` 字串（那會讓空座位變成一個叫 "null" 的人）。
3. **陣列先問 `.IsArray` 再 `.Count`**（codebase 既有慣例，見 `UCL_ReadingLibraryIO` 的 aliases 走法）。
4. 判準：**一個「少一個推薦」等級的降級，跟「這條路完全不通」在骰面上長得一樣。**
   ⇒ 骰面那種每天被讀的產物，降級時該在產物裡留一句話。
