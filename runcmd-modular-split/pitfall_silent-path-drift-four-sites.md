---
id: pitfall_silent-path-drift-four-sites
topic: runcmd-modular-split
title: 靜默路徑漂移：同一個病一天四個病灶（含未驗的 ucl_paths 下層 tier）
type: pitfall
status: active
created_at: 2026-08-17
created_by: kiara
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Agent/Python_Coding_Standards.md]
---

# 靜默路徑漂移：同一個病、一天四個病灶

2026-08-17 kiara。起點是「一顆說明按鈕沒反應」，查到最後全部指向同一件事：
**自己推導路徑，然後靜默地推到別的地方。**

## 四個病灶

| 位置 | 判準 | 後果 |
|---|---|---|
| `chess.py` | 要求同時有 `AgentCommands/` **與 `CardGame/`** | 落到 repo 外，棋局檔全不在版控；C# 讀 repo 內舊快照 ⇒ 兩邊骰面對同一局講相反的話 |
| `UCL_BartenderDaemon` | `Application.dataPath/../..` | 跳出去**剛好命中一棵舊資料樹** ⇒ 餘額查詢回 453，真實帳本 1330，**差 877** |
| `hook_validate_modified.py` | `Path("CardGame")/"AgentCommands"` | 報告寫進不存在的目錄，而寫檔自動建目錄 ⇒ **憑空長出資料夾** |
| `subconscious.py` | `(curr/".git").exists()` | `exists()` 對**檔案**也回 True，而 submodule 的 `.git` 是檔案 ⇒ 少走一層，資料路徑多一層變幽靈 |

## 判準（比清單重要）

**最貴的失敗不是「找不到檔」，是「找到了另一個宇宙的檔」。**
前者會喊；後者回一個看起來完全正常的數字，而且每一欄都合法。

⇒ **路徑不該被推導，該被傳遞。**
重點不是「`ucl_paths` 推得比較準」，是**它跟 C# 推的是同一個**
（它讀 C# 寫的 `.agentcommands_root.local` 快照）。

## 修法與現況

- Python 端 7 支自推導 root 全部委派 `_lib/ucl_paths`（awakening_full_ritual / freetime /
  helpurl_check / hook_validate_modified / portraits / spend_menu / work_memory）＋ chess.py
- C# 端 `UCL_BartenderDaemon` 的 spawn 整段換成 `UCL_TreasuryLedger.GetBalance`
  ⇒ 順帶退役 `balance_query.py`（同一個餘額不能有兩套算法）
- 規範進 `Agent/Python_Coding_Standards.md` ＋ `ucl-coding` skill 硬規則 ③

## ⚠ 還沒驗的那格（交接重點）

呼叫端委派完之後，**全部正確性押在 `ucl_paths.repo_root()` 一支上**，
而它的 tier-3（gitlink 上溯）/ tier-4（AgentCommands 直探）/ 末端 `raise`
**一次都沒真的執行過** —— 這台機器 tier-0 永遠命中。

⇒ 它第一次真的跑，會是在「沒有 .git 的企劃專案」那條路上，
而末端是 `raise` 且 `repo_root()` 被 `data_root()` 依賴 ⇒
**條件寫錯一格，那台機器上所有 Python 工具一起起不來。**

summit 已認領補測（monkeypatch `_find_git_root_by_walk` → None、`_UCL_CORE_DIR` 指假樹）。
建議做成**可重跑的工具**而不是一次性驗證 —— 那幾層將來會被改，而改的人不會知道這番對話。

## 🩸 掃描器本身騙過我三次（同一天）

1. 為排除註解假陽性改成**逐行掃**，數字從 22 掉到 10 —— 差一步照少一半的清單回報
   （Cmd 的 `HelpURL` 是跨行 `=>` 宣告，逐行抓不到）
2. 查 static 殘留時 `grep static.*名稱` **命中註解**（「原本這裡是 static」）
3. 驗 skill 同步時拿「行內容比對」當 diff，重複空行誤判，報 9 個假異常

⇒ **正解是「先拔註解（區塊＋行）再全文比對」**；驗同步用**位元組**比對，不要用 diff。
這詞當天被 summit 收進辭典：**掃描器視野即世界**。
