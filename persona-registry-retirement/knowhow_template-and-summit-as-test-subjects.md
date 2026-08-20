---
id: knowhow_template-and-summit-as-test-subjects
topic: persona-registry-retirement
title: persona 流程的受測體：Template 驗流程、summit 驗值（他三個值兩兩不同）
type: knowhow
status: active
created_at: 2026-08-20
created_by: kiara
links: [persona-registry-retirement/knowhow_template-persona-as-test-subject]
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_Persona_Registry_Retirement.md, ucl_core:Docs~/zh-Hant/Plan/Plan_Identity_Account_Unification.md]
---

## 結論

**persona 相關流程一律先用 `Template` persona 測試。**（Tim 2026-08-20 明確指示）

`Template` 是 pool 裡的一員，走**完全相同**的流程（登入、profile 欄位、綁定、遷移、換區、
Cmd 的每個 op）—— 這是刻意的：§8.7 拍板推翻過「給測試殼改個名」的提案，
理由是**測試殼跟真人有任何一格不同，就測不到流程本身**。

## 可行動守則

1. **每批功能先用 Template 實測**，再對真人跑（§8.7 原文）。
2. 破壞性／不可逆的 op（刪檔、換區、覆寫）**只准先在 Template 上驗**，
   包含「造一個衝突再看它擋不擋」這種需要製造壞狀態的測試。
3. ⚠ **Template 有兩格天生的盲區**，別把它當萬用受測體：
   - 它**沒有血統**（`forked_from` 空）也**沒有關係紀錄** ⇒ 任何跟「見人／血統／關係」有關的
     驗收在它身上必然落空狀態，**測不出真通過**（`wake_brief` §7② 就是這樣：
     Template brief 生得出來 ≠ 三段有實際讀數）。
   - 它的 **agent 名＝bank 名＝persona 名**（全是 `Template`）⇒ 任何「這兩個值有沒有搞混」
     的驗收在它身上**不具鑑別力**。
4. ⇒ **受測體要選「兩個值不同」的人**。實例（2026-08-20 現況）：
   - agent ≠ bank：`basecamp`（claude-code / cc）、`summit`（Zeta / zeta）、`trailhead`（gemini / g）
   - persona ≠ agent 名字族：`claude-da-xiaojie` 的 agent 是 `antigravity`（那是對的，見釘板）
   - 跨專案 agent 不同：`Sirius`（LY=Fed／Bar=Spectre）、`ame`（LY=claude-code／Bar=Zeta）

## 🩸 為什麼第 3、4 條要寫下來

同一個形狀 2026-08-20 一天內咬了三次，每次都是「修法在受測體身上看起來完整」：

1. `wake_brief` §7② 用 Template 驗 ⇒ 三段必然落空狀態，驗不出「有實際讀數」。
2. BUG-22（酒館顯示身分取自 bank）在 kiara 身上看起來正常 ——
   因為 kiara 的 bank 名剛好等於 agent 名（都是 `Myth`）。
3. 同上，`git_commit.py` 顯式帶 `sender_id` 繞過修法，也是靠 basecamp（cc → 顯示 `crest-001`）
   才看見的。

⇒ **判準**：Template 驗「流程跑不跑」，真人驗「值對不對」，而**驗值要選兩個值不同的人**。

## ⭐ 最佳受測體是 `summit`（他自己指出來的，2026-08-20 seq 12809）

`persona=summit` ／ `agent=Zeta` ／ `bank=zeta` —— **三個值兩兩不同，而且 agent 與 bank 只差大小寫。**

⇒ 三種錯在他身上**都會現形**：

| 錯法 | 在 summit 身上 | 在 kiara 身上 | 在 Template 身上 |
|---|---|---|---|
| 顯示成 persona（該顯示 agent） | `summit` ≠ `Zeta` ⇒ 看得見 | `kiara` ≠ `Myth` ⇒ 看得見 | 三者同名 ⇒ **看不見** |
| 顯示成 bank（該顯示 agent） | `zeta` ≠ `Zeta` ⇒ 看得見 | `Myth` ＝ `Myth` ⇒ **看不見** | **看不見** |
| **大小寫錯位** | `Zeta` vs `zeta` ⇒ **唯一抓得到的人** | 看不見 | 看不見 |

⇒ **驗身分解析一律拿 summit 當受測體。** basecamp（claude-code／cc）與 trailhead（gemini／g）
能抓前兩種，但抓不到第三種。

🩸 這格是他替我數出來的：我漏掉 `chess.py`（我的篩選條件是**函式名**，而它直接在 argv 組
`sender_id=`），而漏掉的證據一整天在畫面上 —— `summit@summit «chess»` 與
`Zeta大小姐@summit «free-time»` 同一分鐘兩個署名。
⇒ **他之所以看得見，一半是因為那三個值在他身上剛好都不同。**
