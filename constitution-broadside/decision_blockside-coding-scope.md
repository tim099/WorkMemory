---
id: decision_blockside-coding-scope
topic: constitution-broadside
title: 改 Blockside 先開 Coding 場：要 build 的人 scope 含 index.html，範圍重疊就開 worktree
type: decision
status: active
created_at: 2026-10-16
created_by: ame
links: [constitution-broadside/pitfall_shared-worktree-mixed-build]
related_docs: [scp_core:Runtime/Cmd/SCP_Cmd_Coding.cs]
---

## 拍板（Tim 2026-10-16）
改 Blockside 一律先開 Coding 場、宣告施工範圍；**範圍重疊就開 worktree**。

## 規則
- 認領時帶範圍：`senate cmd task --arg op=claim … --arg scope=<路徑|路徑…>`（或 `senate cmd coding --arg op=start … --arg scope=…`）。⛔ 認領不帶 scope 不會開場，只是「記錄我在做」。
- **要 build 的人，scope 一律含 `D:/Unity/Valhalla/Blockside/index.html`**。
  index.html 是把整個 src/ 一起建進去的產物：兩個人改的 js 不重疊，誰在共用工作區 build，誰就把另一個人的半成品包進去（見 pitfall_shared-worktree-mixed-build）。路徑閘只看路徑，看不出這件事 ⇒ 用「產物也宣告」讓 build 的人彼此互斥。
  import／reimport 結尾會自動 build ⇒ 一樣算。
- 進不了場（範圍跟別人重疊）⇒ 開 worktree，在自己那份裡改、在那裡 build。
  合回 main 前先核 HEAD：main 動過就照新 HEAD 重建 index.html（2026-10-15 cb113a7 照舊底建，蓋掉了 kotoko 16a5507）。
- 只用 FreeTime 場的人也一樣：要動 Blockside 就另開 Coding 場（2026-10-16 混建那次，建成品的人當時只有 FreeTime 場，閘根本沒機會叫）。

## 退場
Blockside 是 JS，`coding op=end` 的編譯閘找不到工程 ⇒ 一定紅燈。
自驗（src/js 全部 `node --check`、`admin.py build`、無頭對拍）後 `--arg force=1 --arg force_reason=<讀數>` 退場；理由會落檔。
做完就退 —— 宣告 index.html 的場掛著，別人就不能在主工作區 build。
