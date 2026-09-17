---
id: pitfall_interactcount-any-default-and-runtime-state
topic: hscene-editor-rework
title: 配對欄位的預設值不是 Any／執行期狀態靠的是沒掛 SerializeField／單維度時看不出來的軌鍵錯誤
type: pitfall
status: active
created_at: 2026-09-17
created_by: kaguya
links: [hscene-editor-rework/decision_interactcount-track-split-20260917]
related_docs: [Assets/Scripts/HScenes/HSceneAssets/InteractCountEvent.cs, Assets/Scripts/HScenes/HSceneAssets/BodySetting.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/ExtensionMethodCore/UCL_AssemblyExtension.cs:822]
---

## ① 兩個 Entry 的預設 ID 都不是 `Any`，而且都指向**真的存在的資產**

    ContectEntry()   ⇒ "LeftHand"   ← 那個 ID 真的存在 ⇒ 安靜地只吃左手
    ClickTypeEntry() ⇒ "Click"      ← 安靜地只吃單擊

⇒ 任何新的配對欄位一律**顯式**寫 `new(AssetAny.ID)`。
不寫的話新門檻會安靜地只數「左手單擊」，而後台看起來一切正常。
📌 這是 `EffectPresetSetting` 2026-09-15 踩過的同一個坑，本次只是換了一個消費端重演。

## ② 執行期狀態放 private 欄位**靠的是沒有 `[SerializeField]`**，不是靠它是 private

`UnityJsonSerializable` 存的是「public 欄位 ＋ **掛了 `[SerializeField]` 的**非公開欄位」
（`UCL_AssemblyExtension.GetAllFieldsUnityVer`，2026-09-17 逐行讀過）。

⇒ `m_TrackCounts` / `m_TrackStamps` 今天不進存檔，**是因為沒有那個 attribute**。
⛔ 誰哪天順手替它補上，下一局就會帶著上一局的次數開場，
而症狀是「事件在開場莫名其妙放了」—— 資產檔合法、數字合法，沒有任何一層報錯。
📌 跟 `InteractCount` 當初不能寫成 `public int` 欄位是同一個理由，但**判準的形狀不一樣**：
那邊是「別寫成 public 欄位」，這邊是「**別加那個 attribute**」。

## ③ 一個能編譯、能跑、測起來全對的錯，只會被第二個需求撞出來

單維度版本（只有 clickType，鍵用觀察值）在它自己的射程內**完全正確**。
它不是被測出來的，是加上 `contect` 時才壞的。
⇒ 判準：**帶萬用字元（Any）的篩選條件，在「觀察值當 key」的表上沒有對應的桶。**
單一維度時 `Any` 剛好可以用「總數」頂替，所以看不出來。

## ④ 已知缺口（明天要驗的兩格，⚠ 目前沒有讀數）

· 兩條軌真的分開數
· 同一條軌被多筆門檻共用時一次互動只推一格

⛔ 而「怎麼驗」要先確認量法存在：**目前沒有次數的可觀測入口**（沒有 DebugOnGUI、沒有 log）
⇒ 要驗得自己加暫時 log 或下中斷點。別假設後台看得到 —— 2026-09-16 寫過一次
「開場景讀 EffectService.DebugOnGUI」而那支東西全 repo 不存在。
