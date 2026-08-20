---
id: state_2026-08-21-shipped
topic: tavern-read-layer-csharp
title: 已上線：兩支 service ＋ 兩個 op ＋ 後台截斷設定
type: state
status: active
created_at: 2026-08-21
created_by: summit
links: [identity-account-unification]
related_docs: []
---

## 這是什麼

酒館的**讀取層**從 python 搬進 C#（Tim 2026-08-20/21 拍板三條：catchup 進 Cmd／query 也轉 Cmd／
**訊息邏輯抽 static class，不放在 Cmd 內**）。

## 現況（已上線並實跑）
- `UCL_TavernQueryService` —— rooms / tail / search / by_sender / timeline / stats / seq
- `UCL_TavernCatchupService` —— 在線＋未讀＋inbox 組簡報；**`AppendUnreadSection` 是未讀段的唯一實作**
  （叮與自由時間換骰共用）
- 入口：`Cmd_Tavern op=query --arg kind=…` / `op=catchup`（薄殼，不含邏輯）
- python 兩支已成**指路 stub**（exit 2），不是刪除 —— Tools 是跨專案共用 submodule，
  各專案 UCL_Core pointer 獨立，直接刪會讓沒 bump 的專案在叮的第一步 FileNotFoundError

## 為什麼搬（不是「比較乾淨」）
「已讀到哪」原本有三個寫入端（C# `UCL_TavernCursor` / python `tavern_cmd.py` / `tavern_catchup.py`）。
2026-08-16 觀影 sidecar 的兩隻游標 bug（從沒設過 ⇒ 從全庫最舊列起／0 筆未讀仍前進 ⇒ 跳過整段）
就是這個家族，而兩次都「看起來很正常」。

## 設定（後台可調，不寫死）
`UCL_ChatTavernSettings.MessageBodyClip`(600) / `MessageBodyClipMentioned`(1500)；
@我 判準＝body 含 `@<persona>`（**不認顯示名／agent 名** —— 那兩個會變，persona 才穩定），
命中的行標 🔔 讓規則在畫面上看得見。

## 🩸 血證
1. **`Get`/`Set` 無條件套筆數 `Clamp(1-500)`**，而 `return defaultValue` 那條路徑沒被夾
   ⇒ 同一欄位「沒設過 600、設過 ≤500」兩種上限，零報錯。已改成可傳夾取規則。
2. **恆亮警告**：截斷警告判準寫成「總則數 ≥ 每房上限」⇒ 59 房加總即恆亮。
   加警告時該問的不是「會不會漏喊」，是**「它在什麼情況下不喊」**。（已入 glossary）
3. **inbox 條目判準**：用「行首是 `- `」會把 commit 訊息的 bullet 數進去（報 42、實際 45）。
   認標題行 `## [seq=…]` 才對 —— 而 42 那個數字看起來完全正常。
4. python 版 `tavern_query stats --since 6h` **逾時 2 分鐘跑不完**（無快取）；C# 版秒級。

## 還沒收的
- per-message 走訪實作 python 三份（handshake / query stub / catchup stub）＋ C# 一份 ——
  `tavern_handshake.py` 檔頭自己寫著「日後應收斂」
- `debuglog_query.py` 在 Bar 無資料可讀（`DebugLogs~` 不存在，那是 CardGame 的東西）；
  `screenshot.py` 在 Bar 零產出。兩支都**沒動** —— Tools 跨專案，刪要兩邊都確認
