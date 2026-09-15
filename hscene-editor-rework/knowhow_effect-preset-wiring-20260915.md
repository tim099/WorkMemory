---
id: knowhow_effect-preset-wiring-20260915
topic: hscene-editor-rework
title: 特效預設組實裝：規則掛章節、上限按預設組記帳，＋三格資產/底層落差與兩格未實測
type: knowhow
status: active
created_at: 2026-09-15
created_by: kaguya
links: []
related_docs: []
---

## 現況（2026-09-15 落地）

`HGameTouchSetting.effectPresets` 在今天之前**零消費端** —— 宣告了、企劃選得到，而演出完全沒反應。
今天補上 `EffectService`（怎麼演）＋ `EffectPresetSetting`（何時放）＋ 接進 `ContactService.CycleCore`。

## 拍板

- **觸發按部位**（Tim）—— 跟表情／人聲對齊，⛔ 不開「互動區域」那一欄
- **規則掛章節不掛資產**（Tim 與熊汁討論後）—— 資產欄位一個都不動，觸發條件包在外層
  · 理由：表情／人聲的場景欄位是**單一**預設組（規則住資產＝等同每章一套），
    而特效是**清單**（可多顆）⇒「這 N 顆裡哪一顆何時出」資產內部放不下
- **同時上限＝整個畫面的預算**（熊汁 ④）⇒ runtime state 以**預設組 ID** 記帳，兩條規則指同一顆共用
- **命中幾條放幾條**（熊汁 ③「一起出」）⇒ ⚠ 與表情／人聲的 `FindSpecial`（先出現者勝）**語意刻意不同**

## 🩸 三格「資產長得像 A、底層是 B」的落差（服務就是那道橋）

1. **兩個 lifetime 管同一件事**：`m_AliveTime`（佔名額）vs `SpineAnimConfig.m_Duration`（播放秒數）。
   磁碟現況是 `Duration:0`＋`Loop:True` ⇒ **動畫不會自己停，一定要有人清**。
   ⇒ 判準：AliveTime 管名額、Duration 管動畫；服務在 `AliveTime + ClearTrackDelay` 清 subtrack。
2. **「同時 N 顆」的單位是 (骨架, subtrack) 槽不是物件**：`AsyncSpineAnim` 不生成實體，
   同一個槽再寫是**覆蓋**。⇒ 上限按槽算、抽選跳過已佔用的槽，
   否則「同時 2 顆」實際是同一顆被蓋兩次，而畫面上只看得到「怎麼只有一個」。
3. **`m_ExcludeLast` 的身分沒了**：原本比對 `SpineEffectObject.name`，而那個 class 已是死碼
   （commit `34b6ddd0b` 把 `m_Objects` 改成 `m_Anims` 時沒清）⇒ 改用**索引**，
   ⛔ 不用 `ToString()`（設定相同的兩筆會被當同一顆）。已於 2026-09-15 移除該死碼。

## ⛔ 兩格只有推斷、**沒有實測**

1. `effectPresets` 從 `List<EffectPresetEntry>`（裸字串陣列）換成 `List<EffectPresetSetting>`（物件陣列）
   ⇒ 舊的 `"effectPresets": ["NewFx1"]` 讀不回來。**Tim 拍板不遷移**（那筆今天不產生任何行為）。
2. ⭐ **更要緊的**：`NewFx1.json` 的鍵是 `"Objects"`，而現行欄位 `m_Anims` 的 JSON 鍵是 `"Anims"`
   （`FieldNameUnityVer` 只脫 `m_`）⇒ **那顆特效的動畫清單很可能整個載不進來**。
   同樣出自 `34b6ddd0b` 那次改名，資料沒跟。

⇒ **一次就能量完兩格**：開場景看 `EffectService.DebugOnGUI` 印的「規則 N 條｜anims M」。
   M=0 證實②，規則數=0 證實①。

## ⚠ 給下一個人的兩句

- `CycleCore` 現在是 **6 支效果的扇出點**（Flag／興奮值／表情／人聲／特效／互動次數）。
  再加東西之前想一下要不要收成清單 —— 但那是獨立的一件事。
- 三個 key 欄位**全部顯式 `new(AssetAny.ID)`**。⛔ 不要照抄 `SpecialVoiceSetting`（它只修了 clickType）：
  `HbodyEntry()`＝`"Default"`（**沒有這個資產**）、`ContectEntry()`＝`"LeftHand"`、`ClickTypeEntry()`＝`"Click"`
  —— 三種「沒設定」的失效都不會叫。
