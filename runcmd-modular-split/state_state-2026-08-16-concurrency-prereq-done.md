---
id: state_state-2026-08-16-concurrency-prereq-done
topic: runcmd-modular-split
title: 併發前置完成、路由仍關（basecamp 接手後狀態）
type: state
status: active
created_at: 2026-08-16
created_by: basecamp
links: []
related_docs: []
---

接續 summit 的交接（state_state-2026-08-16-concurrency-routing-handoff）。本筆是我做完前置後的狀態。

## 已 ship：b45d053（UCL_Core / Dev）

- per-cmd context（`UCL_AgentCmdContext.cs`）：以 cmd id 為鍵，由 runner 注入的 `args["_cmd_id"]` 顯式索引
- 退役的全域槽：`s_CurrentCmdOutputs` / `s_CurrentCmdValues` / `Cmd_Tavern.LastPostSeq`
- `PropagateCmdId(父,子)`：in-process 呼叫另一個 Cmd 時顯式攜帶
- 舊多載刻意刪除 ⇒ 漏改的呼叫端是編譯錯誤（本次 60 個漏網點全由編譯器指出，不是用眼睛掃的）
- montage tag 串 persona ＋ 新增 `KillAllByTagPrefix` 掛在停播（SetRecordingEnabled off）

## ⛔ 仍未做（下一個接手的人從這裡開始）

1. **`AUTO_ROUTE_BY_ARG_PERSONA` 仍為 False**。翻它＝打開併行。前置雖已做，但：
   - **併發下值不會串，至今零現場讀數** —— 編譯 0 錯只證明型別接得上
   - summit §7 的規矩：翻那一行要有第二個人驗，宣稱的人不能是唯一證人
2. `run_cmd.py` 三件未動：移除 `--agent-id`（Tim 已拍板一律 `--persona`）／投遞後回讀 queue 驗證（lost update 從靜默變可見）／arg→lane 反推
3. `CurrentCmdId` 與 `UCL_TreasuryLedger.CurrentCallerEnvMarker` 仍是 static（已收進 context 欄位，尚未替換呼叫端）

## 🩸 會影響修法選型的兩條

- **AsyncLocal 在本專案不可用**：UniTask 不在 await 邊界複製 ExecutionContext ⇒ 兩條並行流共用主執行緒同一份 ambient 值。探針 `UCL_AgentCmdScopeProbe.SelfTestConcurrent` 可重跑，讀數 `A:LEAK,LEAK | A.seq=0(want 1) B.seq=2`。
- **單流測試對併發性質零證據力**：`SelfTest` 幾乎全 HIT，而其中 `backOnMain=HIT` 的原因正是「根本沒隔離」。

## 判準（給驗收用）

路由打開後的紅路：兩人同時 cycle，各自回傳檔裡的 `post_seq` 與回傳檔路徑**都必須是自己的**。
拿到別人的值不會報錯 —— 那是這隻的全部危險所在。
