---
id: pitfall_retrieval-shape-and-gaps
topic: memory-three-layers
title: 回憶檢索的實測特性與三個缺口 —— 句子不是關鍵字、標題行同分噪音、無 persona 過濾
type: pitfall
status: active
created_at: 2026-08-17
created_by: calli
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Memory_Common_Principles.md, ucl_core:Tools~/AgentCommands/kb_targets.json, ucl_core:Docs~/zh-Hant/UCL_EditorPage/UCL_KnowledgeBaseAdminPage.md]
---

## ⚠ 輸入形狀是**句子**不是關鍵字（calli 2026-08-17 實測，同一份索引只改查詢形狀）

| 查詢 | 形狀 | 正解排名 | 分數 |
|---|---|---|---|
| `劇透` | 2 字關鍵字（**該碎片 tags 裡就有這個詞**） | **第 7** | 0.5421 |
| `來源判定` | 只存在於 tags 的詞 | **不在 top-4** | — |
| `呼吸距離` | **正文原句節錄** | **不在 top-3** | — |
| `陪看的時候我把本來就知道的東西當成畫面上看到的講出來，害對方被劇透了` | 完整句子 | **top-1** | **0.7389** |

⇒ **關鍵字查失敗的樣子是「查不到」—— 跟「這條記憶不存在」長得一模一樣，所以它不會叫。**

`Memory_Fragment_Backfill_Workflow` 的成功實測全是完整句子（0.652/0.729/0.677），**但沒明說這件事**。

## 分數帶（basecamp 2026-07-28 定，calli 2026-08-17 複驗）

`0.65~0.74` 真命中 ／ `0.42~0.65` 灰帶（沾到但不是這條，**或該回填的訊號**）／ `≤0.42` 無關。
**index built ✓ ≠ 搜得到** —— 必須抽已知碎片反查 ＋ 跑負向對照，分數帶分離明確才算可信。

## 標題行 chunk 產生同分噪音

`## 一句話` 這種短標題被切成獨立 chunk，內容幾乎相同 ⇒ **實測 6 筆並列 `0.5754`**，
短查詢時整排霸佔前排、真命中被壓在後面。**那不是碎片寫得不好，是 chunk 切法問題。**

## 三個缺口（v1 都沒接，寫進文件的「已知不足」）

1. **無 per-persona 過濾** —— `search` 沒有 persona/路徑參數，`fragments` 的 glob `letters/*/fragments/*`
   那個 `*` 就是 persona 那段，全收。變通 `--format json --topk 40` 事後篩，
   ⚠ **`topk` 是過濾前截斷 —— 自己的碎片排 41 名就永遠看不到，而那個缺席不會叫。**
2. **檢索端不讀 `recurrence`** —— 排序只看語意相似度。所以「多人踩到 ⇒ 更容易被回憶到」
   目前只有兩層落實：recurrence 進 embedding（chunk `#0` 實測 0.6752 可命中）＋ 人自己近分優先。
   要真加權得改 `knowledge_base.py` 排序階段當乘數。
3. **Alaya 無機械索引、無專屬 CLI** —— 碎片還是個位數時，工具會比內容多，刻意不預先造。

## 回填機制（Tim 提案，觸發條件已量化）

> 用一句話查一件**你確定記憶裡有**的事，正解落在**灰帶且排名 > 3** ⇒ 該回填。

做法：把**那句查詢**補進碎片**正文**（開一段「會這樣問」列 2-3 句自然問法），tags 再補中文詞。
⚠ **只加 tags 不夠**（實測 tags 有「劇透」，查「劇透」仍排第 7）。
**回填後必須複驗**（同句再查確認進 top-3）—— 沒複驗的回填等於沒做，**而它看起來完全一樣**。
