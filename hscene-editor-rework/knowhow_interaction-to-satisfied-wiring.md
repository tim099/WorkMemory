---
id: knowhow_interaction-to-satisfied-wiring
topic: hscene-editor-rework
title: 互動↔興奮值接線：這條線早就造好只是零呼叫端；SatisfiedService 是接線層
type: knowhow
status: active
created_at: 2026-09-01
created_by: kiara
links: []
related_docs: [Docs/API/HScenes/SatisfiedService.md, Docs/API/HScenes/HSceneAssets/SatisfiedSetting.md]
---

🩸 進場先知道這件事：**CharacterState.OnTouch 在 2026-09-01 之前是零呼叫端** ——
興奮值／等級門檻／只升不降／高潮／OnLevelChanged 全都早就寫好了，只是沒有人呼叫它。
所以這項工作不是「做興奮值系統」，是接電。

資料流（觸發點 = ContactService.Cycle，一格＝一次互動，自動播放的每一格也算）：
Cycle → 互動區 id → HbodyAsset → 接觸類型特例 → Config.Pick() → CharacterState.OnTouch
      → 等級 → SatisfiedService.SatisfiedLevel ＋ 寫綁定的 SceneFlag（單向）

知識歸屬（為什麼多一層 service）：
「互動區屬於哪個部位」是場景設定的知識，狀態層不該持有；
「興奮值怎麼算」是數值的知識，互動層不該知道。⇒ 對照表住 SatisfiedService，
興奮值本體/計數/高潮留 CharacterState，OnTouch 改成由呼叫端把成長值算好傳進來。

拍板（一律不做遷移）：
- 對照表 key ＝ 互動區 id 字串（不是 ClickAreaAsset —— bodySettings 綁的本來就是區域 id）
- HbodyAsset 特例 key 由 InteractionEntry 改 ContectEntry（舊 key 是另一個維度、永遠不會命中）
- 單次成長值下界夾 0（不夾則「基準2 浮動3」會抽出負值＝互動反而扣分）
- 等級保存在 SatisfiedService 的欄位；興奮值本體仍在 HGameValueAsset
- GameInit 走 ResetSubtracks 語意的清法：只清狀態不動畫面（清軌會讓沒有 Flag 的骨架開局空白）

⚠ 執行期零實測（2026-09-01 收工時）：自動播放速度✕漲幅手感、
m_ContectConfigs 換 key 後編輯器畫不畫得出來、存檔前檢查會不會在正常資料上誤報。
