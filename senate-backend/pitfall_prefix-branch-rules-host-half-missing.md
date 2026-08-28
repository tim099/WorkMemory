---
id: pitfall_prefix-branch-rules-host-half-missing
topic: senate-backend
title: 啟發式家規那一半宿主從沒宣告（UCL_→Dev）＋repo 目標改可直接打路徑
type: pitfall
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/decision_submodule-page-decide-half-2026-08-28]
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

接續同主題的 decision_submodule-page-decide-half-2026-08-28。Tim 當面追加兩點需求，並在對 LY 實跑時抓到一個移植缺口。

**① repo 目標改成可直接打路徑的欄位（預設 Senate 自己）**
原本只能從 senate.local.json 的專案清單挑下拉 —— 而那台機器根本還沒有 senate.local.json（沒跑過 senate init），下拉裡只有 Senate 自己一項。這頁最常見的用途本來就是操作**別的** repo（Senate 是後台，要整理的是 Unity 專案），只能挑清單等於把主要用途擋在設定之後。
改成 TextField（id `submodule/root`，預設 m_Model.RepoRoot）＋「↩ 改回 Senate 自己」鈕（只在真的不是自己時才畫）；設定檔有專案時才畫下拉，而那個下拉是**動作不是狀態**（選了就 SetField 進欄位，自己不持有值 ⇒ 單一真相源）。
路徑比對走 SameRepo/NormRepo（分隔符／尾斜線／大小寫正規化）—— 不然三種寫法會被判成三個不同 repo，那顆鈕對著同一個 repo 一直出現。
目標不是自己時印一句**事實**不是警告（操作別的 repo 是正常用法），但那句話必須說「這個欄位會被 session 記住，下次開頁還是它」。

**② 🩸 啟發式的家規那一半，宿主從來沒宣告 —— 只有對 LY 跑才顯形**
`SCP_GitSubmodule.PrefixBranchRules`（資料夾名前綴 → 追哪條 branch）是 calli 刻意設計成「預設空、由宿主宣告」的，doc comment 甚至寫了範例 `Add("UCL_", "Dev")`。**而 Senate 從來沒宣告過**（全 repo 零呼叫端）。
症狀：`Assets/Plugins/UCL_Core` 目前在 Dev，啟發式算出 master（走到「其餘 → master」）⇒ 顯示 `⚠Dev / 目標 master`。不會切錯（checkout 的祖先檢查會擋下），但那一顆**永遠對不齊、永遠被跳過**，而「被跳過」的訊息看起來完全像盡責。
⇒ Program.Main 補宣告。修後 `✓Dev / 目標 Dev`，同 repo 的 `Assets/Plugins/SCP_Core` 維持 master（SCP_ 不命中 UCL_ 前綴，沒誤傷）。
📌 一般形：**機制在共用層、規則在宿主，兩半都要有人接；只接一半的症狀不是報錯，是一個看起來合理的錯答案。**
⚠ 這是寫在碼裡的家規：要對非 UCL 系 repo 停用就拿掉那一行；需要 per-repo 家規時正解是搬進 senate.local.json（反射三層會自動畫欄位）。**根治其實在 LY 那邊** —— .gitmodules 寫 `branch = Dev` 是 git 原生欄位（解析第二層），任何工具都吃，不必每個工具各自宣告一次。

**對 LY 的實跑讀數（唯讀，沒動任何東西）**
24 顆 submodule 全列出，含三層巢狀（AgentCommands/ChatTavern/baton/letters/kiara）與多 remote（⇈ github.com / gitlab.private / origin）。指令跟著長出 `--root "D:/Unity/LY"`。
順手量到的 LY 現況：AgentCommands 追的分支叫 `LY`（啟發式規則②命中，因為只有一條）；UCL_Core 在 Dev 且 gitlink 與父層不同（父層還沒 bump）；dirty 的有 AgentCommands / letters 四人 / Tasks / WorkMemory / UCL_Core。

**順手修的文件過時**：Cli_Reference 的 `--page` 清單漏了 submodule（實作端 Program.cs Usage 有，文件沒有）—— 正是 calli 在 1fb3f4e 講的「寫死的清單，加頁時不改就會安靜過期」。

**驗收**：build 0/0；selftest 14 過 0 失敗 1 跳過；文字驅動逐格驗過（路徑欄位、改回自己鈕按下後自己消失、24 顆表格、指令閉環）；六個改動檔零隱形字元。
