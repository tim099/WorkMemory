---
id: decision_clickarea-color-and-count-2026-08-26
topic: hscene-editor-rework
title: 顏色綁定 id、MaxValue→Count 改名、預覽切換三格：名字比事實大或小
type: decision
status: active
created_at: 2026-08-26
created_by: calli
links: []
related_docs: []
---

今天在 ClickArea / SceneFlag 這條線上三件事，共同的形狀是**名字比事實大或小**。

**① ClickAreaColorAsset（顏色→區域 id 綁定，新增）**
- 只有一個 m_Color 欄位；**asset 的 ID 就是區域 id**（不加第二個 id 欄位＝不製造第二個真相源）。
- 比對鍵抽成 `ClickAreaColorAsset.ColorKey()`，掃描端與綁定端**共用同一個函式**。⚠ 不用 `Color ==` 比浮點：色彩選擇器的 0.6627451 與 GetPixel 的 0.66274511814 在 hex 上同一格、在 == 上不是，而那不會報錯。
- 表是全域 static（Tim 拍板），刷新開頭 RebuildTable 一次 —— 逐像素查表必須 O(1)，在迴圈裡載資產＝每像素一輪 IO。
- 拍板：A2 命中就覆寫既有 id（只填新色的話已刷過的資產永遠套不到）／B 同色多宣告該色整個不進表並 LogError／C1 全域。
- ⚠ A2 會斷 `ClickAreaRef.id` 的參照，而 `Mathf.Max(0, IndexOf(id))` 會把斷掉畫成「選中第一個區域」。目前只有「當場喊」（逐筆 LogWarning），**預告式對帳沒做**。

**② SceneFlagSetting.MaxValue → Count**
Tim 回報「自動產生的 MaxValue 比預期多 1」。病灶是 Spine 匯入把 `names.Count`（個數）寫進語意是 0-based 上限的欄位。修法是**改名不是改算式** —— 改完那行字面上就對了。附帶換到一格表達力：`Count == 1` 表示「只有值 0」，而 `MaxValue == 0` 會被讀成「未設限」。
⚠ 全案唯一剩下的 `+1` 在 HSceneAsset_EditorImportAreas（素材給的是最大值，轉個數只能在那一處加）；把它搬到 max 外面就是這次修掉的錯的鏡像。
順手：值個數原本忽略 valueAnims（只走新式「值→動畫組」的 Flag 拿到 0 ＝ 未設限，上限靜默消失）；刪掉沒人用又把 count 說成「可接受最大值」的 AnimFlagConfig.Max。

**③ 預覽切換「有時候沒反應」**
既有修法（Play mode 圖與值組同源）複審正確、零回歸，但同族三格沒清：
- `-1` 與 `0` 畫同一張 ⇒ 第一次按 ▶ 看起來沒反應。改成 `-1` = 真的「當下生效那張」，編輯器讀不到 Flag 值就顯示第 1 筆**並在標籤講出來**。
- 索引超出清單長度時顯示端偷偷退回第一筆 ⇒ ▶ 永遠不動、◀ 要按三次（m_PreviewIndex 活在被 cache 的實例上，刪清單不重設）。加 ClampPreviewIndex 在畫按鈕前夾回並寫回。
- 索引解析與 empty-ID→null 各兩份 ⇒ 抽成 GetCombinationByPreviewIndex / NonEmpty。
Condition 分支兩個不對稱也收了（沒有「未設圖」提示、`areaTexture.ID` 的 NRE 窄縫）。
⚠ 標籤字串與 Play mode 的 `-1=當前` 路徑**沒驗到**（要 GUI context ／場景真的有 SceneFlag）。

**驗收方式（可重用）**
私有成員走 `Cmd_Invoke --arg nonPublic=true`；回傳值讀 **Unity Editor.log**（`%LOCALAPPDATA%/Unity/Editor/Editor.log`）—— 專案的 Simulation log 只收 WARNING/ERROR，Debug.Log 讀不到。
