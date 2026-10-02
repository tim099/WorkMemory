---
id: pitfall_layout-regroup-shared-classes
topic: hscene-editor-rework
title: HSceneAsset 排版重排：共用分組類別、反射 scope 路徑、TypeInspect 不證順序
type: pitfall
status: active
created_at: 2026-10-02
created_by: summit
links: []
related_docs: [Docs/API/UCL_Asset/HSceneAsset.md]
---

HSceneAsset／HakoniwaAsset 排版重排（LY e63ebc2f9）踩到的坑：
- **兩個資產共用同一批分組類別**（HGameValue/View/Touch/InteractionSetting）—— 在類別之間搬欄位，箱庭的排版與資料一定跟著變；要動之前先問「箱庭要不要一起」（10-02 Tim：一起改、名字對齊）。
- **反射字串路徑編譯器抓不到**：`UCL_AssetEntryScopedReflect` 的 ScopeType＋ScopeMemberName（ContectHSceneEntry=Values.contects、HGameValueAsset=Values.hGameValues、InteractionHSceneEntry=View.interactionSettings、SkeletonGraphicHSceneEntry=Scene.skeletons）。它們走**屬性名**（Values／View…），所以只改序列化欄位名（values→sceneBackground）不會壞；**搬組**才要改。壞的樣子是下拉選單安靜地退回全體或變空。
- 序列化欄位名 ≠ 存取屬性名：sceneBackground↔Values、sceneConfig↔View。外部程式一律走屬性。
- `HSceneAsset_EditorImportAreas.cs` 有一個不相干的 `AreaImportCell.values`（List<int>）—— 全文取代 `values` 會誤傷，要用 `(?<![.\w])values\.sceneFlags` 這種錨。
- ⚠ 驗收：`ucmd run TypeInspect` 的欄位清單**按字母排序**，只證得了「哪組裝了哪些」，證不了編輯器順序 —— 順序要開 Editor 看。
- 中文名稱只寫在 Docs/API/UCL_Asset/HSceneAsset.md §2（Tim：不做 Localize，編輯器顯示英文欄位名）。
