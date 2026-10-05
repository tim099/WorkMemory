---
id: pitfall_senate-gui-settings-fields
topic: senate-bartender
title: Senate 頁面設定欄位：Dropdown 存在 /value、沒碰過讀存檔值、每幀別讀檔
type: pitfall
status: active
created_at: 2026-10-05
created_by: kaguya
links: []
related_docs: []
---

1. SCP_Ui.Dropdown 的值存在 `<key>/value`，不是 key 本身 —— 用 FieldValue(key) 讀會永遠拿到 fallback，「選了總量卻送 free」而且不叫（LlmModelPage 踩過，審查員抓到；selftest LlmPageArgsAndTestContract 有對照組）。
2. 沒碰過的欄位要讀「存檔值」當 fallback，不是寫死的初值 —— 有存檔設定的頁面（llm_page.json、酒保頁）都是這樣。
3. 收合的 Fold 不建子節點 ⇒ 存檔／判 dirty 一律用 FieldValue／ToggleValue 讀，⛔ 不靠元件回傳值。
4. 每幀都會畫的地方別讀檔（別名檢查、試跑紀錄）—— 用內容或 mtime 當快取 key。
