---
id: state_state-2026-08-17-v1-shipped
topic: memory-three-layers
title: v1 全部落地（7 筆 commit）＋ 五條 pending
type: state
status: active
created_at: 2026-08-17
created_by: calli
links: [worldlines-parallel-memory]
related_docs: []
---

## 已落地（四層 commit，全部單層未 bump）

| repo | SHA | 內容 |
|---|---|---|
| `letters/calli` | `a0a6008` | calli 7 筆個人碎片 ＋ 機械索引（本櫃第一次有 fragments） |
| `AgentCommands` | `5ecfd33ec` | Alaya 第一筆 `lesson_no-spoilers` |
| `UCL_Core` | `63c4874` | 共通原則 ＋ Alaya workflow ＋ `ucl-memory` skill ＋ manifest ＋ kb_targets |
| 主專案 | `0ccd26f1` | 三份 skill 副本 |
| `UCL_Core` | `d9dd3a7` | **門檻修正**（一個人認為就整理，人數＝權重） |
| `AgentCommands` | `98ff47574` | Alaya recurrence 語意 ＋ 自由時間 10 顆像素 |
| 主專案 | `6629c7f3` | 三副本同步 v1.1 |

## 驗收（實跑，非推測）

- Alaya 索引 1 檔 → 25 chunks；真命中 **0.7402**、負向對照 **0.3837** ⇒ 分數帶分離明確
- 端到端 `--target fragments,alaya`：Alaya 前 3（0.6469/0.6359/0.631）＋ `calli/lesson_seen-vs-known`（0.6196/0.6158）
  ⚠ **落在灰帶不是真命中帶** —— 照自己寫的規則那就是該回填的訊號，**還沒做**
- skill 四份 parity 一致（走 `install_skills.py`，`.agents` 的注入 `trigger:` 行正確）

## pending（明天接手）

1. **回填實驗還沒做** —— 端到端那組 0.61~0.64，回填後要複驗進 top-3
2. **`Plan_Memory_Recall_System.md` 沒寫** —— Tim 前一天討論的內容不在 calli 的 context，
   要他講重點或給酒館 seq
3. **`recurrence` 加權沒接**（code 活，`knowledge_base.py` 排序階段）—— 酒館已點名 summit
4. **等同事測試回報**（酒館 seq 11889 發出、11899 更正）：
   句子 vs 關鍵字在他們碎片上是否成立／有沒有確定寫過但撈不到的／33 個觸發詞會不會撞 `ucl-work-memory`
5. **Alaya 只有 1 筆** —— 酒館列了四組既有近似碎片給同事認領
   （`philosophy_conclusion_restraint` mit/summit、`relation_colleague-ecosystem` TakanashiKiara/kiara、
   `identity_storyteller_watchdog` summit/mit、「外觀 OK ≠ 真的 OK」家族）

## calli 個人碎片現況（7 筆，供跨層 link）

`lesson_honest-current-state`(6) / `lesson_engine-vs-fuel`(5) / `lesson_calibrate-not-doubt-theatre`(3) /
`lesson_seen-vs-known`(2, links → `alaya/lesson_no-spoilers`) / `philosophy_true-count-not-beautified`(2) /
`unsolved_no-blade-for-respected`(1) / `identity_reaper-apprentice-recorder`(3, internalized)
