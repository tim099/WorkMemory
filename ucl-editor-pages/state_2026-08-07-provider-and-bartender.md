---
id: state_2026-08-07-provider-and-bartender
topic: ucl-editor-pages
title: ProviderCore 體系 + 酒保時間規則改多行 provider
type: state
status: active
created_at: 2026-08-07
created_by: unknown
links: []
related_docs: []
---

Tim 2026-08-07 派單，UCL_Core 端全部落地（commit ce75625 / 584eee2）。

## 新增 ProviderCore 體系
- `UCL_StringProvider`（抽象基底）+ `UCL_StringValueProvider`（預設實作）+
  `UCL_StringBookRecommendProvider`（隨機推薦藏書，Editor-only）。
- 宣告欄位務必 `[SerializeReference]` —— 多型的唯一觸發訊號，少了會靜默丟子類資料。
- 推薦 provider 放 `EditorCore/.../Books/` 而非 ProviderCore：它依賴 Editor-only 的
  UCL_BooksIO，放 runtime 層會層級倒置且 build 編不過。代價：build 後 runtime 還原不回該型別。

## 酒保時間規則改造
- `reminder_msg`(string) → `reminder_lines`(`List<UCL_StringProvider>`)，
  廣播走唯一組裝入口 `GetReminderBody()`（逐行求值後以 \n 串接）。
- 舊格式遷移實作在 `UCL_BartenderTimeRule.DeserializeFromJson()` override，
  判準是「新欄位缺席」而非「舊欄位存在」。
- 序列化 triggers / time_rules 兩者 Load 與 Save 都走 UCL.Core.JsonLib；
  state / assignments 維持 JsonUtility。
- 移除 target_id + HP penalty 整套（含 daemon Pass 3、FirePenaltyWarning、
  inline parser 的 target=/grace=/penalty=、RegisterTimeRule 五個參數）。

## 閱讀卡轉發 + 早安 §6.6
- `SyncBookshelf` 額外轉發一份到 `letters/<persona>/bookshelf/<media-id>.md`。
- `wake_brief.py` 新增 §6.6 見書，隨機端一張閱讀卡（deterministic 種子
  `persona:bookshelf:wake_count`），只讀 letters 不碰 Library。

## pending
- LY 層安裝副本與 AgentCommands 的 time_rules.json 尚未提交（單層指示）。
- `install_skills.py` 黏字修正需重跑安裝才會套用到其餘 7 個 skill。
