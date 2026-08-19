---
id: state_progress-2026-08-19-phase12
topic: bartender-llm
title: 酒保接本機 LLM —— Phase 1/2 落地、Phase 3 未動
type: state
status: active
created_at: 2026-08-19
created_by: basecamp
links: []
related_docs: []
---

## 現況（2026-08-19 basecamp；UCL_Core 7636ce0 / efb4e1d / 89f1c35，父層皆未 bump）

**已落地**
- 模型管理頁 `UCL_LLMModelAdminPage`（ToolBox 入口）＋ `UCL_LLMAdminRunner` ＋ `llm_admin.py`（ollama 薄層，唯一真相源）
- 目錄含 Qwen 系列與大模型；下拉預設只列 `vram_gb ≦ 6`（門檻 `VRAM_BUDGET_GB` 在 python，C# 只顯示 —— 兩份門檻必漂）
- 一鍵安裝 ollama（官方 `install.ps1`，開**可見** PowerShell）／🧹顯存卸載／⛔中斷（kill python ＋ `ollama stop` **兩段**）
- 試跑分四區（報告／回覆／思考／紀錄）＋ append-only `AgentCommands/LLMAdmin/test_log.jsonl`（**連 system prompt 一起記**）
- 酒保頁「發言來源」：罐頭（預設）／已安裝模型下拉、閒置卸載 120s、生成與等待上限、人設 prompt
- daemon 接上 `@酒保`：偵測 → 節流 → 生成或罐頭 → post；meta 帶 `reply_source` 與 `triggered_by_seq`

**實測讀數**
- `@酒保` seq 12502 → 12503 走 `qwen3:0.6b`（「你被點名了，對吧？」）
- 冷卻：連發 12506/12507 **只回 12506**（`replied_seqs` 落磁碟）
- `qwen3:4b` 思考吃 1000–3700 token／50s；`0.6b` 3s／20 tok，但夾簡體與粵語「嘅」

**未動（Phase 3）**
- 出口簡繁／語域轉換 —— 目前真的會吐簡體
- 酒保頁的 mention UI（冷卻／每日上限／罐頭池只能改 `llm_settings.json`）
- 罐頭 fallback **零讀數**；每日上限與 `s_Running` 閘只有邏輯
- `vram_gb` 回填實測值（只有 4b 對到 ollama 回報的 3.2GB）

**坑（都寫進 commit 訊息了）**
- 頁面沒傳 `--timeout` ⇒ python 用預設 60s，4b 實測 50s **卡在邊界隨機失敗**
- `Refresh()` 覆寫 `m_Report` ⇒ 試跑結果被蓋（**同一個坑在 UCL_AutoCommitPage 已踩過一次**）
- bool 被序列化成字串（`"enabled":"True"`）⇒ python 讀 `"False"` 是 truthy；已 override `SerializeToJson` 修正
- **模型是 ollama 服務持有的**：Process 管理頁空著 ≠ 顯存還回來了（唯一解是 `ollama stop`）
