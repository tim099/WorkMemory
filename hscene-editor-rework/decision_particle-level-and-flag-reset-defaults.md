---
id: decision_particle-level-and-flag-reset-defaults
topic: hscene-editor-rework
title: 粒子等級設定與 SceneFlag ResetGame：兩個功能的預設值各有一個不會叫的失效樣子
type: decision
status: active
created_at: 2026-09-21
created_by: kiara
links: []
related_docs: []
---

2026-09-21 kiara 交付兩個功能，兩個的**預設值**都是這次真正該記的東西。

## ① ParticleSetting.levelSettings —— 依興奮度等級調整粒子（commit 88314e002）

`List<ParticleLevelSetting>`：`active`（預設 true）＋ `rateOverTime`（預設 **-1**）。
清單索引 ＝ 興奮度等級（0-based），越界夾到最後一筆 —— **照 FaceExpressionPresetAsset.PresetLevelAnims
的既有慣例，⛔ 不造第三套等級索引規則**。

⚠ `rateOverTime` 預設刻意是 **-1（＝不調整，沿用 prefab 原值）不是 0**（Tim 拍板）：
若預設 0，設計師只想在某一級關開關而順手新增一筆條目時，會**靜默**把該級噴發量歸零 ——
失效樣子是「粒子物件開著、演出照跑、就是不噴」，零報錯零 log。

🩸 **寫入端衝突（動手前量到的，別再繞回去）**：`gameObject.SetActive` 已經有第二個寫入端——
`EnableParticle`（AsyncEvent，演出腳本用）。
⇒ 規則明寫：`ParticleService` 訂閱 `CharacterState.OnLevelChanged`，
**只在等級變動與 GameInit 那一次套用，⛔ 不是每幀覆寫**，兩者 last-write-wins。
每幀覆寫會把演出那條路靜默吃掉，症狀是「事件跑了、log 乾淨、粒子就是不出現」。
⛔ 別為了「等級才是權威」改成每幀。

## ② SceneFlagSetting.resetOnGameReset（commit 1128a6340）

⚠ **這不是加一個開關關掉既有行為，是把那個行為做出來。**
改動前 `SceneFlagService` **只覆寫 GameInit、沒有 ResetGame**，而 `HGameBase.ResetGame`（:665）
跑的是 `service.ResetGame()` ＝ 基底空實作 ⇒ **按 Reset 時場景 Flag 一格都不動**，
而其他服務都真的重置了 ⇒ 畫面上分不出來、零報錯。

⇒ 預設 true 是**行為改變**（Tim 拍板）。射程已量：Test.json 19 筆＋Test2.json 3 筆＝**22 筆**受影響。

**兩條重置路刻意不同語意，⛔ 別為了看起來一致合併：**

| | 射程 | 值 | 鎖定 state | 看開關 |
|---|---|---|---|---|
| `GameInit()` | 整場重新開始 | 還原 | **還原** | ❌ |
| `ResetGame()` | 這一局重來 | 還原 | **不動** | ✅ |

把 ResetGame 改成呼叫 GameInit 會把鎖也還原掉；GameInit 不看開關也是刻意的
（否則新開一局會帶著上一場的值，而那在畫面上看不出來）。

📌 初始值的 lazy-cache 抽成 `CacheInitValue()`：它現在有**兩個**入口（GameInit 與 ResetGame），
兩邊各寫一次遲早只有一邊被改到，而分岔症狀是「重置回到的是上一輪的值」——**跟正確行為同形**。
