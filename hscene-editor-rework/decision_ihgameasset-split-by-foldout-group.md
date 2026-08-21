---
id: decision_ihgameasset-split-by-foldout-group
topic: hscene-editor-rework
title: IHGameAsset 由 17 個單清單介面改為 6 個分組介面（子設定物件）
type: decision
status: active
created_at: 2026-08-21
created_by: Sirius
links: []
related_docs: [Docs/API/UCL_Asset/HSceneAsset.md, Docs/API/UCL_Asset/HakoniwaAsset.md, Assets/Scripts/UCL_Assets/HGameAssetSettings.cs]
---

IHGameAsset 原本聚合 **17 個單清單介面**（IInteractions / IClickAreas / IAreaEvents / ISliderSettings…），
2026-08-21 改為 **6 個分組介面**，介面抓的是子設定物件而不是清單。

## 量到的現況（改之前，機械掃描非目測）
- 17 個介面裡 **9 個在宣告檔外零使用**（IHControlAssets / IClothSettings / ITimmingEvents /
  IOnUpdateEvents / IAreaEvents / ISliderSettings / IRightButtonSettings / IButtons / IParticles）。
- 真正被用的 8 個，用法幾乎都是 `is X aScope` / `as X` 的**能力探針**（SpineAnimRef / SetSceneFlag /
  SceneFlagService / TriggeredEventService…）—— 窄介面在那些點是資產不是負債，所以「介面太細」不是問題本身。
- **真正的重複在資產本體**：HSceneAsset 與 HakoniwaAsset 各自手寫 **27 個同名同型欄位**
  （＋多數還各有一個同名 property），而分組資訊只活在 HSceneAsset 的 `[UCL_FoldoutGroup]` 上，
  Hakoniwa 那邊一個都沒標 ⇒ 兩邊天生會分歧。

## 拍板（Tim 2026-08-21）
1. **不做 migration**，舊 JSON 直接壞掉 —— 整個專案正在打掉重構，不用考慮舊資料。
   （當時全庫只有 3 個資料檔：HSceneAsset/Test.json、Test2.json、HakoniwaAsset/Hakoniwa.json）
2. **HakoniwaAsset 目前沒在用 → 直接與 HSceneAsset 共用同一組子設定型別**，不保留任何差異。
   結果差別只剩場景 Prefab 載入源（`hScene` vs `scene`）。

## 落地形狀
子設定型別在 `Assets/Scripts/UCL_Assets/HGameAssetSettings.cs`，分界**直接沿用原本的 FoldoutGroup 分段**：
`HGameSceneSetting` / `HGameInteractionSetting` / `HGameEventSetting` / `HGameValueSetting` /
`HGameViewSetting` / `HGameTouchSetting`；介面 `IHGameScene` / `IHGameInteraction` / `IHGameEvents` /
`IHGameValues` / `IHGameView` / `IHGameTouch`（＋沿用 `IHSceneConfig`、`UCLI_ID`）。
⇒ 分組從「顯示層的 attribute」變成「型別本身」，兩張資產不可能再分歧；加一個清單只動那一個子 class。

副產物：HGameBase 與 HSceneEditorWindow 裡 **3 組 `Asset is HSceneAsset ? … : Asset is HakoniwaAsset ? …`
三元分支整段消失**（共 11 個欄位的取值），因為兩張資產現在共用 `Touch` 等子物件。
