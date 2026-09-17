---
id: decision_interactcount-track-split-20260917
topic: hscene-editor-rework
title: 互動次數門檻按 contect✕clickType 分軌：軌鍵是篩選組合不是觀察值（Tim 2026-09-17 拍板）
type: decision
status: active
created_at: 2026-09-17
created_by: kaguya
links: []
related_docs: [Docs/API/HScenes/HSceneAssets/InteractCountEvent.md, Docs/API/HScenes/HSceneAssets/BodySetting.md, Assets/Scripts/HScenes/HSceneAssets/BodySetting.cs, commit:b6784db6d]
---

Tim 2026-09-17 拍板：**「每個分別判定＆記數」** —— 門檻加上 `contect ✕ clickType` 兩個配對 key（皆預設 `Any`），
而它們是**計數的分軌鍵**，不是「達標那一下的門票」。

    大腿掛 A={2 次, Any✕Any} 與 B={5 次, Any✕Click}
    單擊兩下 ⇒ A 達標、B 才數到 2，繼續往 5 累積

⇒ 「被右手滑滿 5 次」寫得出來。⛔ 不是「總數滿 N 且這一下是該組合」——
那種設計下前 4 下單擊 ＋ 第 5 下滑動也會觸發「5 次 Slide」，**而後台顯示完全一樣**。

## 🔑 軌鍵是「門檻自己的篩選組合」，不是「觀察到的組合」

這一格我**先做錯過一版**（只有 clickType 時鍵用觀察值），加上第二個維度才露餡：

| 做法 | 「`Any` 接觸 ✕ `Click`」要讀哪個數字 |
|---|---|
| 觀察到的組合當 key | ❌ 沒有。左手單擊在 `(LeftHand,Click)`、右手單擊在 `(RightHand,Click)`，它要的是兩者的**和** |
| 門檻的篩選組合當 key（現行） | ✅ `(Any,Click)` 那一格 |

⇒ 累加是「**逐筆門檻問它配不配**」（`InteractCountEvent.MatchTrack`），
⛔ 不是「把這一下的組合丟進表裡 +1」。

## ⚠ 這個做法自己帶來的第二個失效點（已擋，別拆掉）

篩選組合相同的多筆門檻**共用同一條軌**（「Click 5 次」與「Click 20 次」本來就該讀同一個數字）
⇒ 逐筆推的話那條軌一次互動被加兩次，症狀是**設 20 次的東西第 10 次就觸發**，
而沒有任何一層會說那是重複計數。
⇒ `BodySetting.m_TrackStamps` ＋ `m_InteractStamp` 判重，同一條軌本次只推一格。
📌 那不是保險，是這個做法的**必要配件** —— 誰把它當冗餘刪掉，門檻會提早一半觸發。

## 落點
· `InteractCountEvent`：`contect` / `clickType` / `MatchTrack` / `TrackKey` / `HasTrack`
· `BodySetting`：`m_TrackCounts` / `m_TrackStamps` / `OnInteract(contectID, clickTypeID)` / `GetInteractCount(track)`
· 接線：`ContactService.CycleCore` → `SatisfiedService.OnInteractCount(areaID, contectID, clickTypeID)`
