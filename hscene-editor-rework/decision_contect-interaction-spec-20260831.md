---
id: decision_contect-interaction-spec-20260831
topic: hscene-editor-rework
title: 互動判定與觸發：Tim 2026-08-31 全套拍板（區域 id／不分方向／0=off／收手四時機／滑動節奏／疊加）
type: decision
status: active
created_at: 2026-08-31
created_by: calli
links: []
related_docs: [Docs/API/HScenes/HSceneAssets/ContectSetting.md, Docs/API/UCL_Asset/ClickTypeAsset.md, Docs/API/UCL_Asset/HControlAsset.md]
---

2026-08-31 Tim 拍板，互動判定與觸發（ContectSetting → SceneFlag）這條線的規格全部定下來：

## 資料與判定
- **互動區的身分就是區域 id**（同 id 視為同一區，跨分色圖合併）⇒ `ContectSetting.m_ClickArea`
  由 `ClickAreaEntry`（分色圖資產 ID）改為 `ClickAreaRef`（區域 id）。
- **互動不分方向**（企劃確定規格）—— 不是漏掉。所以 `ContectTypeSetting` **不加** `CheckDirectionSetting`，
  滑動也不顯示方向箭頭。⚠ 我曾把「欄位不存在」讀成「相對舊路的 regression」並提議補欄位，那是誤讀。
- **清單順序 = 優先序**，first-hit 早退。兩層 `ConditionGroup` 都要留（reference 原始 Setting，
  不是單獨 reference condition）。
- 區域判定一律用 `initAreas`（按下時所在區域）⇒ 滑動滑出原區不會斷；
  游標預演用 `curAreas`（`checkInput=false`）。

## Flag 語意
- **0 是 off，不是循環的一格** ⇒ `Cycle` 走 1..Count-1 繞回 **1**，永遠不經過 0。
  用 `% Count` 每繞一圈會閃一次 off 姿勢。
- **收手 → Flag 切回 0**。收手只有四條路：①這一區被別的互動搶走 ②本互動換到別區
  ③`HControlPanel.StopAnim` ④場景重置。**放開滑鼠不在這四條裡** —— 互動不會因放手而結束。
- `TurnOff` **不套任何閘門**（含 decreaseCondition）：閘門擋的是玩家調值，收手不是玩家的調整；
  擋下去的後果是手收了、動作還在演，而且沒人會再更新它。

## 速度與疊加
- 速度 1 = 每秒一格、速度 2 = 每 0.5 秒一格（間隔 = 1/速度）。實作用累積（`timer += dt * speed`）
  而非倒數，中途改速度不殘留舊間隔。
- **滑動觸發與左側面板自動播放疊加**，不互斥（拍板）。⇒ `Begin` 對 Slide **刻意不歸零** `iGroup.timer`，
  否則「自動間隔 > 滑動間隔」會永遠等不到自動那一次 —— 那是靜默互斥。

## Slide（滑動）
- 命名不用 Continuous/Repeat（跟自動播放的週期性重複撞語意）、不用 Swipe（UI 慣例是一次性快劃）。
- 啟動距離**重用 `dragDistanceMin`**，不另開欄位（同義的第二個真相源遲早分岔）。
- 節奏：越過啟動距離**當場觸發一次**（初次觸發）→ 每 `m_SlideInterval` 判一次
  「這段期間位移 > `m_SlideDistance`？」→ **不論觸發與否都開新窗口** → 放開即中斷。

## 一個順手修掉的行為變更（會咬人）
`dragDistanceMax` 的比較方向原本與 Min 逐字相同（都是 `dragDis < 門檻 → false`）
⇒ 它實際上是第二個 Min。而 `Press.json` 填 `dragDistanceMax: 15`，意圖是「按久但沒滑走」
⇒ 修正前的長按要求「按超過 0.3 秒**且拖超過 15px**」。已修成 `>`。
⚠ 資料一格沒改，只有判定方向改了 —— 所以行為變更**不會有任何提示**。
