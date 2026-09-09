---
id: knowhow_interaction-settings-moved-to-scene
topic: hscene-editor-rework
title: 互動的可變設定從 asset 層搬到 scene 層（2026-09-09 四刀）＋ 三格編譯器不會叫的接縫
type: knowhow
status: active
created_at: 2026-09-09
created_by: calli
links: [hscene-editor-rework/pitfall_scoped-reflect-member-path-silent-fallback]
related_docs: [Assets/Scripts/HScenes/HSceneAssets/InteractionSetting.cs, Assets/Scripts/UCL_Assets/InteractionAsset.cs, Docs/API/UCL_Asset/InteractionAsset.md, commit:eba1bf513]
---

今天（2026-09-09）Tim 派的四刀是**同一個方向**：把「這個互動在這個場景怎麼用」從 asset 層搬到 scene 層，並把散開的判定閘門收攏成唯一入口。四刀分開看像四件雜事，連起來是一條線。

## 四刀與它們的共同形狀

| # | 改動 | 搬走的是什麼 |
|---|---|---|
| ① | `HGameInteractionSetting.interactions`（裸 `List<InteractionEntry>`）退場，改由 `HGameViewSetting.interactionSettings`（`List<InteractionSetting>`）供給 | 「場景支援哪些互動」的**真相源** |
| ② | `InteractionAsset` 的 `m_Contects` / `OnEnterEvents` / `OnExitEvents` / `m_Condition` / `m_SetAsDefault` 整段廢棄，搬進 `InteractionSetting` | 「同一個互動在**這個**場景怎麼用」 |
| ③ | `SceneFlagSetting` 三道閘門收攏成受限入口 `ApplyValueGated`，`Cycle` 改走它 | 「准不准改這個值」的**判定位置** |
| ④ | `ContactService.WarnMultipleContinuous` 判準從「幾個持續型別」改成「幾個搶同一份 `ContinuousState`」 | 「什麼叫衝突」的**定義** |

⇒ 共同判準：**跨場景不變的留在 asset，逐場景不同的搬到 scene 層。**
asset 是 `UCL_Asset`，同 ID 全案共用一個 cached instance ⇒ 掛在它上面的設定等於所有場景被迫共用一份，而那些設定本來就該逐場景不同。

## ⚠ 三格「編譯器不會叫」的接縫（下一個動這塊的人一定會撞）

1. **`InteractionHSceneEntry` 的 scope 是 reflection 字串路徑**
   （`ScopeType` / `ScopeMemberName` / `ElementIDMemberName`）。
   ① 那一刀要三處一起動：`IHGameInteraction`→`IHGameView`、
   `"Interaction.interactions"`→`"View.interactionSettings"`、**新增** `ElementIDMemberName => m_Interaction`
   （元素從裸 entry 變成 `InteractionSetting`，ID 藏進 `m_Interaction`；resolver 走 `ExtractElementID` 的
   `UCLI_ID nested` 分支，而 `UCLI_AssetEntry : UCLI_ID`）。
   🩸 **漏任何一處都沒有編譯錯誤 —— 失效樣子是 Editor 下拉默默變空**（`ResolveMember` 只 LogWarning 一次）。
   ⭐ **而這一格 @Sirius 2026-08-21 就記過了**：同主題的
   `pitfall_scoped-reflect-member-path-silent-fallback`（「清單搬進子物件會讓 ScopeMemberName
   反射靜默失效 → 下拉退回全體 ID 且不報錯」）。
   ⚠ 我今天是**自己從 code 推一遍**才知道要改那三處 —— 沒有先查工作記憶。
   ⇒ 兩筆已 link。**開工前讀這個主題的 pitfall，比從 code 推便宜。**
2. **`ContactService.m_Contects` 的元素型別靜默換了**（`ContectEntry` → `ContectHSceneEntry`）
   —— setting 上欄位同名、元素也有 `.ID` ⇒ **四處消費端一個都沒報錯**。
   語意驗過：`m_ContactDic` 的 key 是 `ContectSetting.m_Contect?.ID`（contect asset 的 ID），新舊 entry 的 `ID` 是同一個東西。
   ⛔ 但那是**讀 code 的結論，不是實跑讀數**。
3. **json key 與 C# 欄位名不同名**：UCL 序列化剝掉 `m_` 前綴 ⇒ json 是 `Interaction`／`Contects`，
   而 reflection 要用 C# 欄位名 `m_Interaction`。⚠ **不要照 json key 去改那個字串**。

## 📌 資料面：不做 migration（Tim 2026-09-09 再確認）

退場欄位的 key 會留在 json 裡沒人讀（9 張 InteractionAsset 全部殘留 `Condition`／`SetAsDefault` 等）。
⚠ 而 `Touch.json` 的 `Contects` 是**真有內容的資料** ⇒ 要在場景的
`view → interactionSettings → 那一項 → m_Contects` 重設，否則 `ContactService` 拿到空清單而**沒有任何互動命中得了**。
（Tim 當天已在 `Test.json` 設好 `LeftHand`／`RightHand`。）
📌 `Hakoniwa.json` 的 `interactions` 在**根層**（六組分組之前的舊格式）⇒ 新 code 本來就讀不到它，那不是本次造成的。

## 分組語意破了一個口（拍板已知）

`HGameViewSetting` 檔頭原本寫「這一組全部改掉也不會影響任何判定結果」——
`interactionSettings` 決定場景支援哪些互動、每項還帶 `m_Condition` 與進出場事件 ⇒ **那句話不再整體成立**。
現在寫的是「其餘五個欄位仍是純顯示；『互動選不到／按不下去』要查這裡」。
⇒ 六組分組的乾淨切線因此有缺口，要不要另立一組是後續的決定。
