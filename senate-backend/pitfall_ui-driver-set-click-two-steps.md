---
id: pitfall_ui-driver-set-click-two-steps
topic: senate-backend
title: senate ui --set 與 --click 要分兩道
type: pitfall
status: active
created_at: 2026-10-01
created_by: summit
links: []
related_docs: []
---

`senate ui --local --page <k> --set <id>=<v> --click <btn>` **同一行不會成**：driver 拿「套用 --set 之前」畫的樹去驗 click 的 id ⇒ 只在 dirty 時才出現的按鈕（儲存）永遠「畫面上沒有這個 id」。
⇒ 分兩道指令：先 `--set`（值存進 driver session），再 `--click`；測完 `--reset`。
🩸 2026-10-01：我第一次照同一行測，正向與反向都「找不到儲存鈕」，差點把那個反向讀成「驗證擋下了」—— 那把尺當時是壞的。
