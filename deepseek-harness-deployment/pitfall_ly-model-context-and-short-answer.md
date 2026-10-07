---
id: pitfall_ly-model-context-and-short-answer
topic: deepseek-harness-deployment
title: DSH 宣告容量與 Ollama 實際容量分開驗
type: pitfall
status: active
created_at: 2026-10-07
created_by: meadow
links: []
related_docs: [D:/Unity/Senate/Docs/Workflows/DeepSeek_Harness_Local_Deployment.md, D:/Unity/Senate/Docs/Workflows/DeepSeek_Harness_Ollama_Integration.md]
---

換機模型接入不能只複製 DSH 的 capacity。Ollama OpenAI 路由的實際 num_ctx 另由模型 alias／Modelfile 管理；LY 的 qwen3:4b 原本只有 4096，與 DSH 8192 不一致，壓縮後偏題。16K alias 已對齊但 agent 的初始提示仍使輸出預算變小，4B 的思考輸出也未可靠關閉；template 試改未改善，已還原。

本次改用既有 qwen3:0.6b 建立 32K alias，provider 明確 Off → none，DSH 與 /api/ps 均讀回 32768。UI 基本問題 13 秒完整回答 2 加 3 等於 5。這只證明接入與問答，不證明小模型的程式修改／工具循環能力。使用者若要更強模型須另外驗證；不要把「已完成」狀態當答案正確。

CLI loader flags 要放在 app flags 前；自動 UI picker overlay 只供驗收，交付時已移除。LY 的 helper PID 依 port 命名（web-3080.pid），停止前核對實際 executable、完整 CLI path 與 listener，不能沿用 Bar 的 PID 或 web.pid。
