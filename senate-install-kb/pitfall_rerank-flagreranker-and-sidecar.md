---
id: pitfall_rerank-flagreranker-and-sidecar
topic: senate-install-kb
title: 重排實作坑：FlagReranker 壞、舊 sidecar、連線重試
type: pitfall
status: active
created_at: 2026-10-03
created_by: kotoko
links: []
related_docs: [senate:Docs/Workflows/Kb.md]
---

rerank 的實作坑（TASK-0382，2026-10-03）：
- ⛔ 不要用 FlagEmbedding.FlagReranker：它呼叫 tokenizer.prepare_for_model，新版 transformers 已經沒有（`XLMRobertaTokenizer has no attribute prepare_for_model`）。sidecar 改用 AutoModelForSequenceClassification 直接載 bge-reranker-v2-m3：成對輸入、取單一 logit、sigmoid。
- sidecar 的 /health 回 features；舊版沒有 ⇒ Senate 端 EnsureRunningWithRerank 關掉重起（腳本是內嵌資源，重起就換成新的）。🩸 但「在跑、features 有 rerank、而實作壞掉」不會被重起——改了 sidecar 腳本之後要手動 `senate cmd kb --arg op=sidecar --arg action=stop`。
- 評估連打 200+ 個請求時偶發 `connection aborted`（常駐程序還活著）；嵌入與重排是冪等的，客戶端對 HttpRequestException 重試最多 3 次（⛔ 不重試程序回的 500）。
- dev senate.exe 在暫存目錄跑要自帶一份 SenateData/config（含 kb_eval.json）——它讀的是 cwd 底下那份，不是 repo 的；題庫改了要複製過去，不然跑的是舊題庫。
