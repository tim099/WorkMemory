---
id: pitfall_windows-shell-and-verification
topic: deepseek-harness-deployment
title: DeepSeek Harness 跨機部署
type: pitfall
status: active
created_at: 2026-10-07
created_by: meadow
links: []
related_docs: [D:/Unity/Senate/Docs/Workflows/DeepSeek_Harness_Local_Deployment.md, D:/Unity/Senate/Docs/Workflows/DeepSeek_Harness_Ollama_Integration.md]
---

換機部署先看 Senate 的兩份權威文件：Docs/Workflows/DeepSeek_Harness_Local_Deployment.md、Docs/Workflows/DeepSeek_Harness_Ollama_Integration.md。不要把本機 node、pnpm 或模型服務的可用性推定到 LY。

Windows 的 GUI Git push hook 可能由 Git Bash 啟動；互動 PowerShell 能找到 pnpm，不代表 hook 的 PATH 也能找到。錯誤 `/usr/bin/bash: pnpm: command not found` 是本機 pre-push typecheck 尚未執行的證據，不能當成 GitHub 拒收；應在實際 hook shell 驗證 Node/pnpm 路徑與 typecheck，保留 hook。

DSH 的網頁可用、Ollama 列得出模型、DSH 實際完成一輪模型回應是三個不同驗收層次。現有文件明寫前兩者的讀數與第三者未驗，接手時不要把可連線擴張為端到端成功。LlmModelPage 管理的是模型服務；DSH 要另外設定相容 provider 與 `/v1` endpoint。
