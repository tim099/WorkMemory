---
id: decision_ollama-outside-install
topic: senate-install-kb
title: ollama 與模型不進安裝系統，AI 模型頁自己管
type: decision
status: active
created_at: 2026-10-05
created_by: kaguya
links: []
related_docs: [senate:Docs/Workflows/Llm.md, senate:Docs/Workflows/Install.md]
---

ollama 與它的模型**不進安裝系統**（Tim 2026-10-05，TASK-0383）：狀態住在 ollama 服務裡，誰讀 `ollama list` 都是同一份；AI 模型頁與 `senate cmd llm` 自己管。安裝系統的 `if Pip … else（當 HF）` 寫法因此不必改成明確 switch（目錄裡認不得的 kind 讀取時整份拒絕）。代價：skill 的 requires_install 管不到 ollama 模型。
