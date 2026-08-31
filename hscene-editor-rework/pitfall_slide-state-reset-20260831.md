---
id: pitfall_slide-state-reset-20260831
topic: hscene-editor-rework
title: Slide 三症狀：狀態機的重置不能放在「只有在命中路徑上才會被呼叫」的函式裡
type: pitfall
status: active
created_at: 2026-08-31
created_by: calli
links: []
related_docs: [Docs/API/UCL_Asset/ClickTypeAsset.md]
---

2026-08-31 血證：Slide 上線當天被回報「只能觸發第一次」「第二次以上不受啟動距離限制」「放開瞬間被判成 Click」——
**三個症狀，兩個根，全部在 `ContactService.Begin` 與我對「重置該放哪」的誤判上。**

## 症狀 A：只有第一次會觸發
`Begin` 裡兩行各自都合理，合起來互斥：
1. `if (aIsNewBinding) iGroup.Clear();`（換區時關掉舊 Flag）—— 它連 `slideState` 一起歸零，
   而**判定（CheckSlide）已經先跑過了** ⇒ 當場擦掉它剛認定的 `active=true`
2. `aShouldPlay = aIsNewBinding || clicked || IsDrag` —— **沒有 Slide 那一格**
   ⇒ 第二次之後：綁定沒變、沒放開、又不是 Drag ⇒ 判定成立卻不播
⇒ 就算只修一個，另一個還是會讓它只播第一次。**兩個 bug 疊在同一個症狀上。**
修法：`Clear(bool iResetSlide = true)`，只有換綁定那條路傳 false；`aShouldPlay` 加 `IsSlide`
（對 Slide 而言**命中本身就是答案**，CheckSlide 只在真的觸發那一幀回 true）。

## 症狀 B：第二次以上不受啟動距離限制 ← **最貴的那一格**
「放開即中斷」原本寫在 `ClickTypeAsset.CheckSlide` 裡（看到 `!pressed` 就 Reset），
而那支**只在 `Match` 命中那一筆時才會被呼叫**：
- 放開那一幀，`Match` 依清單順序先命中排在前面的 `Click`（first-hit 早退）⇒ Slide 沒被評估
- 再下一幀 `ClickInfo.Clear()` 清空 `initAreas` ⇒ 呼叫端在 CheckAreas 為空時早退
⇒ `active` 永遠停在 true，第二次按下**直接跳過啟動距離門檻**，
  拿新位置去比**上一次手勢留下的** `windowStartPos` ⇒ 幾乎必然 > 5px ⇒ 立刻觸發。

⇒ **判準（這條值得跨專案記）：狀態機的重置不能放在「只有在命中路徑上才會被呼叫」的函式裡。**
判定會被短路，而重置不該跟著被短路。修法：搬到呼叫端每幀無條件執行的位置
（`ContactService.OnClick` 最上面，在任何早退之前）。

## 症狀 C：滑動的尾巴附贈一次 Click
`ClickInfo.Release()` 只在 `eventTriggerTimes == 0` 時設 `clicked` —— 那是既有的
「這次按壓已經有結果了，放開不算點擊」機制，Drag 靠 `OnTriggerEvent()` 達成。
我為了避開它的副作用（清 `prePositions` ＋ 動全域計數器）**整個不呼叫** ⇒ 計數器停在 0。
⇒ 修法是**一次按壓只記第一次**（`if (eventTriggerTimes == 0) OnTriggerEvent();`）：
抑制 `clicked` 只需要計數器 ≥ 1，而每 0.5 秒都 ++ 會把同一次按壓裡 AreaEvent 的 Drag 連帶擋掉。

## 排查過程本身的教訓
我先猜「資料設錯」（不是，`Slide.json` 全對）、再猜「Drag 搶先命中」（不是，那組只有 Click+Slide）、
再猜「像素單位換算」（是真的但不是主因）。**三次都沒中，中的是 Tim 那句「好像是在第二次以上才發生」。**
⇒ 事後補了 `DebugOnGUI` 印 `dragDis / eventTriggerTimes / clicked / slideState` ——
滑動的門檻全是「數字有沒有到」，而它們原本**沒有一個看得見**。

## 附帶：像素門檻的單位
`ClickInfo.GetDragDis` 把 normalized delta 乘上 (1920, 1080) ⇒ 門檻是**虛擬像素**，
視窗不是 1920 寬時跟實際螢幕像素不等比（視窗 960 寬 ⇒ 實際滑 25px 就達到 50）。
不是 bug，但它長得像 bug ——「門檻比想像的鬆」多半是這一格。
