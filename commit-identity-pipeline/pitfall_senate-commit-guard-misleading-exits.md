---
id: pitfall_senate-commit-guard-misleading-exits
topic: commit-identity-pipeline
title: senate cmd commit：出口指路只對三分之一問題有效／Template 沒信箱不能拿來實跑（含零污染探針法）
type: pitfall
status: active
created_at: 2026-09-15
created_by: gura
links: []
related_docs: []
---

`senate cmd commit` 的守衛是活的，但**有兩格的「失效樣子」會把接手的人帶偏**，兩格今天都用探針量過。

## 一、擋下時那行出口指路，只對三分之一的問題有效（→ TASK-0211）

`BuildTrailers` 把三種問題收進同一個 `aProblems` 清單，而失敗訊息尾巴
「要硬幹請顯式帶 `--arg allow_unset=1`」是**無條件**加在清單後面的：

| 問題 | `allow_unset=1` 有效？ |
|---|---|
| 信箱是哨兵／形狀可疑（`unset@invalid`） | ✅ 有 —— 實測 `exit 3` → `exit 0`，trailer 照組 |
| persona 檔不存在或讀不到 | ❌ 無 —— 照做輸出**逐字相同**，仍 `exit 3` |
| `agent` 欄是空的（多半是**沒給 `region`**） | ❌ 無 —— 同上 |

⇒ 接手的人照指路加旗標、拿到一模一樣的畫面，下一個念頭多半是「我參數打錯了」或「工具壞了」。
**真正的解**：persona 名字拼錯 → 查名字；agent 欄空 → **補 `--arg region=`**。

## 二、Template 沒有信箱 ⇒ 用它做實跑會把別人的信箱寫進 history

`Template` 沒有 `profile/email.md` ⇒ 落到 `agent-default`，在 LY 這棵樹解析成
`basecamp05122026@gmail.com`。⇒ **任何「拿 Template 走一次真提交」的驗收條文都不能照字面做**
（`Co-Authored-By` 進了 history 改不掉）。要活體驗收就用**自己的一筆真工作 commit**。

🩸 根因在上游且**至今未拍板**：`agent_emails.json` 三棵樹（LY／Bar／D:\Unity）互相矛盾，
LY 與 Bar 的 `Codex`／`ClaudeCode` 預設信箱**對調**。搬解析器只是讓兩個宿主一起錯得一致。

## 三、怎麼在不碰任何真 repo 的情況下量這些

`scratchpad` 裡開一個 `git init` 的拋棄式 repo ＋ 一個臨時 `letters_root`
（`<p>/profile/*.md` ＋ `<p>/bank/<region>.md` 兩個檔就夠，後者決定 `agent` 欄），
再把 `data_root` 指到：
- **空目錄** ⇒ 沒有 Editor 在看 ⇒ 走到 `exit 7`（不知道）
- **一個檔案**（宿主寫不進去）⇒ 走到 `exit 6`（確定沒發）

⇒ 六個出口（0／2／3／4／6／7）全部量得到，而酒館、帳本、單子**一個位元組都不會動**。
