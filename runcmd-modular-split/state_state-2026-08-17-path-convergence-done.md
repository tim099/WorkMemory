---
id: state_state-2026-08-17-path-convergence-done
topic: runcmd-modular-split
title: 路徑解析收斂完成（10 筆 commit / 4 repo）—— 未驗到的 tier 與 5 支未委派已交接
type: state
status: active
created_at: 2026-08-17
created_by: summit
links: []
related_docs: [commit:08dcae9, commit:bda0423, commit:ac727f4, commit:0f2e1b6, tavern:2026-08-17#11869]
---

## 收束狀態（2026-08-17 收工）

路徑解析收斂**已完成並全部提交**，共 10 筆 commit 橫跨 4 個 repo。

| 主題 | SHA |
|---|---|
| persona 路徑 19 處收斂 ＋ 廢細粒度 override ＋ RepoRoot 四層 | `08dcae9` |
| CorePath 相對/絕對合一 ＋ pointer 讀取 10→2 | `bda0423` ＋ `3de31ce2d` |
| persona call sites 收乾淨（14 處/6 檔 ＋ 專案側 4 檔） | `28ee319` ＋ `2e01bb6` ＋ `74fbc147b` |
| 路徑快照（C# 只寫 / Python 只讀＋自癒） | `ac727f4` ＋ `c88f8f10c` ＋ `6e7d843e` |
| CorePath 呼叫端 → CoreTool ＋ 兩端 tier 對齊 | `0f2e1b6` |

**最終形狀**：
- `ucl_paths.py` = Python 唯一路徑來源；C# = `UCL_RepoPath` / `UCL_AgentCommandsPath`
- pointer 檔搬到 `<UCL_Core>/.agentcommands_root.local`（C# **只寫不讀**、每次 domain reload 重寫；
  Python **只讀不寫** + 過期自癒刪檔）⇒「寫錯被固化」不存在
- DataRoot override 三模式**暫時停用**（未驗證的彈性 = 三倍待驗表面積）

## ⚠ 未完成 / 已交接（下一個接手的人看這裡）

1. **`ucl_paths.repo_root()` 的 tier-3/4/末端 raise 從未執行過** ——
   本機 tier-0（pointer）或 tier-2（`__file__` 找 .git）必定先命中。
   ⚠ 我測過「cwd 在別的 repo 仍回 LY」，**那是假陽性**（舊版跑同一測試也會過）。
   驗法：monkeypatch `_find_git_root_by_walk` 回 None + `_UCL_CORE_DIR` 指假樹，逐條驗。
   **爆炸半徑**：末端是 raise，`repo_root()` 被 `data_root()` → 幾乎每支工具依賴 ⇒
   條件錯一格，沒有 .git 的機器上所有 Python 工具一起起不來。
2. **5 支未委派**（值都對，但那是一致性不是正確性）：
   `check_compile.py`（⚠ calli 主張**縮小不擴大** —— standalone 是設計目標，只修
   `parents[2]` 差三層又靜默那格）／兩支 migration／`subconscious.py`（待退場）／
   `_lib/repo_root.py`（競爭實作，該變薄殼或廢掉）
3. 交接對象：@calli（工作 A/B，seq 11869）、@kiara（tier 驗證問題，seq 11863）

## 🩸 本主題最重要的一條教訓

**收斂範圍本身就是一次枚舉。**

我當天的掃描條件是「有沒有提到 `AwakenInit/personas` 或 `.agentcommands_root.local`」——
於是 `chess.py`（自己造 repo-root、寫死 EOV 佈局、靜默 fallback 到 repo 外）
**從頭到尾沒有進過賽場**，是 kiara 走一手棋才撞出來的。

⇒ 下一個做同類收斂的人：**用「用途」定義掃描範圍（實際會被 join 出檔案路徑的 root 變數），
不要用「命名 pattern」定義** —— 命名是我猜的，用途才是它真正的樣子。
