---
id: pitfall_localize-label-grep-subset-20260911
topic: hscene-editor-rework
title: 改欄位名要對 Localize 字典 —— 用自編 grep pattern 撈會漏，要拿改動清單逐個列舉
type: pitfall
status: active
created_at: 2026-09-11
created_by: kiara
links: []
related_docs: []
---

改欄位名／加欄位時，`UCL_LocalizeAsset/Default.json` 的標籤要跟著動 —— **而找法錯了不會叫**。

## 🩸 現場（2026-09-11，我自己）
把 `voiceKeys` → `Voices`、`ClimaxVoiceKeys` → `ClimaxVoices`，並新增 `clickType` / `condition` / `m_Probability`。
我用**自己編的 grep pattern**（`oice` / `limax` / `pecial` / `asePreset`）去字典裡撈該改的標籤，
回報「兩個標籤跟著改好了」。

當晚改成**逐個列舉我動過的欄位**再去查字典：

```
Voices        -> 聲音清單     ✅
ClimaxVoices  -> 高潮組聲音   ✅
clickType     -> None         ❌ ← 我當天新增的欄位
part          -> None         ❌ （既有缺口）
contect       -> None         ❌ （既有缺口）
```

⇒ **我找的是「名字裡有 voice 的」，不是「我今天動過的」。** 兩者在沒漏的時候輸出完全同形。

## 動作（可搬到別人現場）
改完欄位之後，拿**改動清單**（diff 裡真的動過的欄位名）去對字典，⛔ 不是拿關鍵字去撈。
序列化名 = 去掉 `m_` 前綴（`m_ClimaxVoices` → `ClimaxVoices`），所以對照表查得動。

## 為什麼值得記
缺標籤的後果是後台顯示**英文原欄位名** —— 不編譯錯、不報錯、企劃看到一個陌生的英文字就跳過。
`FaceExpressionPresetAsset` 到今天還留著兩個**已刪欄位**的中文標籤，就是沒人回頭對過的殘留。
📌 同族血證：@basecamp 2026-09-11「讀了一個自己挑的子集，然後當全集報出去」
（sed 開 34-51 行的窗報 15 個未宣告、讀 mentions 只取前 70 行報 4 筆而真值 13）。
