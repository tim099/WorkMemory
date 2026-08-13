---
id: knowhow_import-interaction-areas
topic: hscene-editor-rework
title: Import interaction areas — 分色圖依 <Group>_<N> 自動生成互動區域＋補 SceneFlag（含自動補 Flag 拿掉異源閘門的代價）
type: knowhow
status: active
created_at: 2026-08-13
created_by: apex-one
links: []
related_docs: [Assets/Scripts/UCL_Assets/HSceneAsset_EditorImportAreas.cs, Assets/Scripts/UCL_Assets/HSceneAsset_EditorImport.cs, Docs/API/UCL_Asset/ClickAreaAsset.md]
---

新按鈕：HSceneAsset 的「Import interaction areas」（`aba6aa1e`，主專案）。
實作檔 `Assets/Scripts/UCL_Assets/HSceneAsset_EditorImportAreas.cs`（partial，整檔 UNITY_EDITOR 包住，
必須與 HSceneAsset 同組件所以不能放 Editor 資料夾 —— 與 Import spines 同一條限制）。

## 做什麼
`config.m_InteractionSprites`（此欄位原本全專案零讀取端，是為本功能預留的）→ 掃該 importer 前綴下的
sprite ID → 依 `<Group>_<N>` 分組 → 生成 SceneFlagValue 模式的 ClickAreaAsset（**索引 = Flag 值**）→
綁 SceneFlag（缺就補）→ 註冊進本場景 clickAreas。

推導鏈（全機械）：
```
ClickAreas_Scene2_Pants_3
  → 前綴取自 importer.m_IDFormat 填空  ← 不自己拼「ClickAreas_ + 資料夾名」，那是推導
  → 餘 Pants_3 → group=Pants, index=3
  → 資產 ID = Scene2_Pants（前綴去掉 ClickAreas_）
  → m_FlagImages[3] = ClickAreas_Scene2_Pants_3
```

## Tim 的三個拍板（2026-08-13）
1. **缺值 fallback**：取最近的前一張；索引 0 就缺則取最近的後一張。
2. **SceneFlag 缺就補**（與 Import spines 一致）。新 Flag 的 MaxValue 由素材最大索引推導。
3. 單層 commit，不 bump 父層。

## 代價（第 2 條拍板拿掉了一道閘，這是本主題最該記住的一件事）
第一版設計是**異源對帳**：素材分組名來自美術檔名、SceneFlag 名來自 Spine 動畫名 —— 兩隻手的產物，
對不上就紅燈。自動補 Flag 之後那道閘消失：`Cloths` 對不上 `Clothes` 時不再報錯，
而是**安靜生出一個叫 Cloths 的 SceneFlag（bindingFlags=0）然後一切正常運作**。

現況就有這個實例：`Cloths_0.png` / `Cloths_1.png` / `Clothes_2.png` 三個拼法不一致，
按下按鈕後產出 `Scene2_Cloths`（2 格）與 `Scene2_Clothes`（已存在 → 不動），**兩邊都不報錯**。
正確拼法是 `Clothes`，要修的是**素材檔名**，不是程式。

保留的守衛只有一條、而且是精確比對不是拼字猜測：**只有大小寫不同時仍然拒絕**
（補上去會得到兩個語意相同的 Flag）。

## 其他刻意的取捨
- 已存在的 ClickAreaAsset 一律不動（沿用 EnsureSkeletonAsset 慣例），但**仍補「掛回場景」那一步** ——
  資產在、場景沒引用 = 完全沒有互動，而那個狀態不會報錯（Q14：無生效圖片資料屬正常）。
- **先補 SceneFlag 再存資產**：反了的話新資產指向當下還不存在的 Flag，畫面上是空下拉。
- 按鈕排在 Import spines 之後不是排版：本鈕靠 sceneFlags 當對帳來源，那些 Flag 是上一顆鈕生的。
- 不批次掃色塊：RefreshAreaConfigs 逐像素掃全部分色圖，批次會凍住 Editor；報告改成指路到該資產頁。
- fallback 的三層可見性：dry-run 逐格標 `← fallback 補` / 確認框印補格總數 / 執行後額外 LogWarning
  指名哪幾個值。理由：補完的對照表**看起來就是完整的**，不標的話沒人會發現素材沒畫齊。

## 實跑驗收（Tim 2026-08-13 於 Editor 按鈕，非推測）
```
Scene2_Pants   Flag=Pants   4 格全有專屬素材、0 格 fallback
Scene2_Cloths  Flag=Cloths  自動新建(MaxValue=1, bindingFlags=0)、2 格
Scene2_Clothes 資產已存在 → 不動檔，僅補註冊進 clickAreas
Test2.clickAreas = [Scene2_Clothes, Scene2_Cloths, Scene2_Pants]
```
編譯 0 errors（2026-08-13T16:52:07）。

## 未完
生成的資產 JSON 與 `Test2.json` 尚未 commit —— 那批混有本 session 之前既有的未提交改動
（`ClickAreaSpriteEntry.cs` / `HSceneConfig.cs` / Scene1 貼圖增刪），歸屬待 Tim 判斷。
