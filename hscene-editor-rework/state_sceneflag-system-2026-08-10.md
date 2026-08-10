---
id: state_sceneflag-system-2026-08-10
topic: hscene-editor-rework
title: SceneFlag 系統落地（三道閘門 / ClothSetting 改綁 / ClickArea 值模式）
type: state
status: active
created_at: 2026-08-10
created_by: unknown
links: []
related_docs: []
---

2026-08-10 summit：SceneFlag 系統整套落地（LY 主專案 commit 0ab6ab6f / 6d9ef793 / 2d145487 / 82304234）。

做完的：
- SceneFlagSetting（HSceneAssets）：ID/Value/MaxValue/bindingFlags/valueEvents + 三道可變更閘門
  （condition 通用 / increaseCondition / decreaseCondition，未設定＝視為符合）。
  CanAlter / CanIncrease / CanDecrease / CanAlterValue；到達上下限視為不能增減（Tim 拍板）。
  閘門在寫入之前判（讀的是 Value，先寫再判等於問錯對象）。
- SceneFlagService + ISceneFlags（HSceneAsset / HakoniwaAsset 都實作），HGameBase 註冊。無 GameUpdate。
- SetSceneFlag（AsyncEvent）+ SceneFlagRef；SceneFlagValueProvider（FloatProvider 體系）。
- ClothSetting 改綁 SceneFlag：CanShow/CanPutOn/CanTakeOff；服裝鈕隱藏、穿脫鈕變灰不隱藏。
  nameKey 改為可選（留空退回 flag.ID）。
- ClickAreaAsset 新增 EClickAreaImageMode：Condition(預設,0) / SceneFlagValue（索引＝Flag 值）。
  介面收斂到 CurrentAreaTexture（唯一下游介面），CurrentAreaImage 降級為 Condition 專用。

pending：
- 全部只有 Tim 手動實跑驗收，沒有自動化測試。
- HakoniwaAsset 雖實作 ISceneFlags，但小箱庭只做簡易操作，SceneFlagRef 的 ClickAreaAsset 反查不找它。
- SkeletonGraphicService.AlterAnimFlagValue 目前無呼叫端（ClothPanel 改走 SceneFlagService 後）；未刪。

坑（值得帶走）：
- 「編譯過」對「功能有沒有被接上」是結構性盲區：DrawModeWarnings() 定義寫進去了、呼叫沒接上，
  errors=0。抓到它的是逐項 grep 對帳（符號出現 1 次、應為 2），不是編譯。
- check_compile 會讀到 Unity 的「空編譯」結果（0.2s / 0 訊息 / 0 個 Assembly-CSharp 條目）——
  那不代表編過。可信的是 recompile 自己等完後印出的那一行；或確認訊息裡有 Assembly-CSharp。
