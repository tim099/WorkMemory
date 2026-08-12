---
id: pitfall_uninstall-silent-noop
topic: ucl-skill-install-sync
title: 退場 skill 刪不掉且回報成功 — 候選集只從源端枚舉 → 空集 exit 0
type: pitfall
status: active
created_at: 2026-08-12
created_by: apex-one
links: []
related_docs: [ucl_core:Tools~/install_skills.py, ucl_core:Docs~/zh-Hant/UCL_EditorPage/UCL_AgentSkillManagerPage.md]
---

**症狀**：UCL_Core 移除某個 skill 後，已裝端（`.claude` / `.agents` / `.codex`）的目錄不會消失；
從 `UCL_AgentSkillManagerPage` 的 Matrix 按移除也**看不到那個 skill**；就算手動跑
`install_skills.py --include <該 skill> --uninstall` 也**回報成功卻什麼都沒做**。

**三個洞疊在一起（缺一個都不會這麼難查）**：

1. **候選集只從源端枚舉** —— `selected = filter_skills(discovered, include, exclude)`，
   而 `discovered = discover_skills()` 是 `Skills~` 的 filesystem truth。
   已退場的 skill **定義上只存在於已裝端** → 濾成空集 → 迴圈跑零次 → `removed=[]` → **exit 0**。
   呼叫端（Editor 頁的移除鈕）拿到「成功」。**這是靜默的空轉被記成成果，比失敗難查。**
2. **Matrix 結構上看不見它** —— `DrawAgentMatrixPlaceholder` 迴圈跑的是 `Skills~` 的目錄，
   源端沒有的東西連迴圈都進不去。
3. **agent 載入 skill 不看 `.ucl_source`** —— Claude Code 只掃安裝目錄。
   所以「看不見」的那個 skill **仍然會被吃進 context**。看不見 + 仍生效 = 靜默僵屍。

**修法（`4f9aa15`）**：
- `--uninstall` 候選集改為 **`Skills~` ∪ 已裝目錄**（十行以內的改動）。
- 顯式 `--include` 點名卻沒被移除 → **exit 2** 並印原因。
  判準：**「找不到」與「不需要做」在程式裡長得一樣，但在意圖上是反的**
  —— 「順手掃一圈沒東西」＝成功；「我要你刪 X」而 X 沒被刪＝失敗。
- 新旗標 `--force-remove-unmarked` 才碰無 `.ucl_source` 的目錄；
  **刻意不讓 `--force-overwrite` 兼任**（那顆是「覆蓋內容」，不該多一個「刪除來源不明目錄」的副作用）。
- Matrix 底部新增 orphan 區（空則不繪製；有/無 marker 分橘灰兩色；`DisplayDialog` 二次確認）。

**驗證方式（別靠印象）**：造 sandbox `--project-root`，分別放有 marker / 無 marker 的假 orphan，
比對 `Skills selected: []` 與 `Uninstall selected: [...]` 兩行 —— 舊行為就死在前者。
⚠ exit code 要用檔案捕捉：`cmd > out.txt 2>&1; echo $?`。
走管線的 `cmd | tail; echo $?` 拿到的是 `tail` 的退出碼（我第一次就這樣拿到假的 exit=0）。

**同族**：個人 fragment `apex-one/lesson_absent_things_never_error`（不存在的東西不會報錯）。
