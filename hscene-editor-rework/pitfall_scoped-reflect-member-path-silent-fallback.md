---
id: pitfall_scoped-reflect-member-path-silent-fallback
topic: hscene-editor-rework
title: 清單搬進子物件會讓 ScopeMemberName 反射靜默失效 → 下拉退回全體 ID 且不報錯
type: pitfall
status: active
created_at: 2026-08-21
created_by: Sirius
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/AssetCore/UCL_Assets/UCL_AssetEntryScoped.cs, Assets/Scripts/UCL_Assets/InteractionAsset.cs, Assets/Scripts/UCL_Assets/SkeletonGraphicAsset.cs]
---

把清單搬進子物件時，**最危險的不是編譯錯誤 —— 是那兩處用字串反射抓成員名的下拉選單。**

## 形狀
`UCL_AssetEntryScopedReflect<T>` 用 `ScopeMemberName`（字串）在 asset 型別上找成員：
- `InteractionHSceneEntry`：`nameof(IInteractions.Interactions)`
- `SkeletonGraphicHSceneEntry`：`nameof(HSceneAsset.Skeletons)`

清單降一層之後 `ResolveMember(assetType, "Interactions")` 回 `null` → `GetScopedIDs` 回 `null`
→ 上層 `GetAllIDs` 判 `IsNullOrEmpty` → **退回「全體 ID」**。
⇒ 下拉選單看起來完全正常（有選項可選），只是**不再被場景 scope 限制**，
   而且**一句警告都沒有**。編譯 0 error、GUI 不報錯、選單有東西 —— 三個訊號全綠。

## 修法（2026-08-21，兩層都做了）
1. **讓它不可能靜默**：`UCL_AssetEntryScoped.cs` 的 `ResolveMember` 在找不到成員時 `Debug.LogWarning`
   一次（null 也進 cache，所以不會每幀洗 log）。
2. **讓路徑不可能打錯**：`ScopeMemberName` 改支援點號路徑（新增 `ResolveMemberPath`，逐層取值，
   中途 null 視為正常空狀態、靜默），而呼叫端一律用 `nameof` 組出來：
   `$"{nameof(IHGameScene.Scene)}.{nameof(HGameSceneSetting.skeletons)}"`
   ⇒ 欄位改名時**編譯就會擋**，不會等到有人發現下拉沒被 scope。

## 判準
**搬動任何「被字串反射引用」的成員，先 grep `ScopeMemberName` / `nameof(` / `GetField("`。**
編譯過不代表這類引用還活著 —— 它們的失敗模式是「行為悄悄變寬」，不是「壞掉」。
