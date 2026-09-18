---
id: decision_scene-flag-lock-state
topic: hscene-editor-rework
title: SceneFlag 鎖定狀態（SceneFlagState 四態）—— 閘門要接兩處、TurnOff 不受管、state 序列化且 GameInit 還原
type: decision
status: active
created_at: 2026-09-18
created_by: apex-one
links: []
related_docs: [Docs/API/HScenes/HSceneAssets/SceneFlagSetting.md, Docs/API/IAsyncEvents/SetSceneFlagState.md, Docs/API/ConditionBase/SceneFlagStateCondition.md, Docs/API/UtageExtensions/CustomCommands/AdvCommandSetSceneFlagState.md, commit:bc49e8431]
---

**Tim 2026-09-18 要求**：`SceneFlagSetting` 要能被**鎖住**，且鎖可由 AsyncEvent／Utage 指令切換、由 Condition 判讀。

## 形狀

`SceneFlagState` 四態（`Default` 不鎖／`NoIncrease` 不能增／`NoDecrease` 不能減／`Locked`）：

- **`Default = 0`** ⇒ 既有資料反序列化後全是不鎖，行為與加這欄之前逐格相同。
- 它是**兩個獨立許可折成一個 enum**，⛔ 不是由鬆到緊的等級 —— `NoIncrease` 與 `NoDecrease` 之間沒有大小關係。
- 判讀集中在 `SceneFlagStateExtensions.AllowIncrease()/AllowDecrease()`：⛔ 各呼叫點不准自己寫 `state != Locked && ...`，往 enum 加新狀態時漏改的那份**不會報錯**，只會讓某個入口比別人鬆。

## 閘門接在**兩處**（缺一不可，這是最容易漏的一格）

| 接點 | 誰走它 |
|---|---|
| `CanIncrease` / `CanDecrease` | UI（`ClothSetting.CanPutOn/CanTakeOff`、`HGameBase` debug 按鈕） |
| `ApplyValueGated` | **`Cycle`（互動播放）走這裡而不是 `SetValue`** |

⇒ 只寫前者的話，**循環播放會繞過鎖**，而畫面上看不出來。
與三道 condition 是 AND，而且**鎖先判**（鎖是有人顯式切的，條件是算出來的）。

## 兩個拍板（Tim 2026-09-18）

1. **鎖不管 `TurnOff`** —— 收手／換區／`StopAnim` 不是玩家的調整；擋下它會讓「手收了、動作還在演」，而那個狀態沒有任何人再更新。
2. **`state` 序列化 ＋ `GameInit` 還原成 json 那一格** —— 初始快照抄在**第一次 `GameInit`**（那時它還是反序列化原值），與 `Value` 同構。抄晚了會抄到被事件改過的狀態，而「還原成上一輪的鎖」與「還原成 json 的鎖」在畫面上同形。

## 三個入口一份邏輯

`SetSceneFlagState`（AsyncEvent）／`AdvCommandSetSceneFlagState`（Utage，Arg1=flag 必填、Arg2=state 預設 `Locked`）共用 **`SceneFlagStateSetter`**；反向判讀走 `SceneFlagStateCondition`（`states` 清單 OR，空清單＝無條件通過）。

## ⚠ 已知代價（拍板選的，不是漏做）

`NoDecrease` 會讓 `Cycle` **卡在最後一格** —— 迴繞在數值上是下降，與 `decreaseCondition` 同形的既有代價。

## 落點

`SceneFlagSetting.cs` / `SceneFlagService.cs`（+`SetState`/`GetState`）/ `SceneFlagStateCondition.cs` / `SetSceneFlagState.cs` / `SceneFlagStateSetter.cs` / `AdvCommandSetSceneFlagState.cs`；文件四份＋`DOC_INDEX` 與 `ADD_CUSTOM_COMMAND_WORKFLOW` 已同步。commit `bc49e8431`（LY 單層，⚠ 父層未 bump、未 push）。

## ⛔ 未驗

**只有編譯讀數**（errors 0 / stale 0 / clean）。沒進 Play Mode、沒開「Utage 自訂指令一覽」頁、沒跑 Import。
