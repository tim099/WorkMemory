---
id: decision_voice-preset-wiring-20260911
topic: hscene-editor-rework
title: voicePreset 接上讀取端：三來源擇一、兩道獨立的骰、一把共用冷卻（Tim 09-11 六條拍板）
type: decision
status: active
created_at: 2026-09-11
created_by: kiara
links: []
related_docs: []
---

`HGameTouchSetting.voicePreset`（規格 2.11）2026-09-11 接上讀取端，接線層是新的 `VoiceService`
（掛在 `ContactService.CycleCore`，排在 `SatisfiedService` → `FaceExpressionService` 之後）。
commit `47bd5a081`（LY 主層，6 檔）。

## 拍板（Tim 2026-09-11，逐條）
1. `SpecialVoiceSetting` 補 `clickType` ＋ `condition` ⇒ 跟表情的 `FaceExpressionSpecial` 逐欄對齊。
2. 命中後**隨機抽一支**（不是整組播完）；冷卻**整組共用一把**（三個來源都推同一顆計時器）。
3. 高潮期間：一開始拍「不播」，接上 `m_ClimaxVoices` 之後改成播高潮組。
4. `m_BaseVoices` / `m_ClimaxVoices` **都改用 `PlayVoice`**（原本是 `List<string>` voiceKeys）。
5. `SpecialVoiceSetting.m_Probability` 預設 **50**，**取代**整組機率（⛔ 不相乘）。
6. special 沒抽中 ⇒ **再擲一次整組的機率**，過就播高潮／基礎（兩道**獨立**的骰，不是乘積）。

## 現在的判定順序（改動它之前先讀這段）
`CD 擋` → `FindSpecial`（先查先記，不決定播不播）→ special 命中就擲**它自己**的機率
→ 過就播 special；沒過**落到第二層**再擲整組機率 → 過則 高潮中播 `m_ClimaxVoices`、否則 `m_BaseVoices[等級]`。
⚠ 機率判定**必須在來源選定之後**。反過來寫（先擲整組再選來源）會讓 special 的機率變成
「在整組之上再乘一次」—— 那是拍板**否掉**的語意，而**兩種寫法在單一資產上可能給出一樣的手感**
（`NewVoice1` 兩邊都填 50 時）。

## 還沒接的一欄，以及它缺什麼
`m_ChangePitch` **零讀取端**。缺的不是實作而是**「快慢」的定義**（scrub 速度／互動頻率／
`InteractionDuration` 是三個不同的量）；而且要改 pitch 得動到 `UtageVoiceSetting`
（AVG 事件也在用那支）。**未拍板，不碰。**

## 射程（交接時別誤讀）
整條線**只有編譯讀數**（`Errors 0 / stale_sources 0 / clean`），**沒有任何執行期讀數** ——
沒有人真的摸過一下、聽到一聲。debug 面板的 `VoiceService` 那行印
`last:<聲音名>(special|climax|base LVn)`，進場摸幾下就看得出走哪一組。
