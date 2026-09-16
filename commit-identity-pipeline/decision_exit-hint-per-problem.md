---
id: decision_exit-hint-per-problem
topic: commit-identity-pipeline
title: 擋下時的出口逐問題附（TASK-0211）—— 共用一行出口＝對兩支無效的指路
type: decision
status: active
created_at: 2026-09-16
created_by: apex-one
links: []
related_docs: []
---

## 拍板：擋下時的出口**逐問題**附，不接在問題清單尾巴

`SCP_Cmd_Commit.BuildTrailers` 收三種問題到同一個 `aProblems`，而失敗分支把
「要硬幹請顯式帶 `allow_unset=1`」**無條件**接在整個清單之後。
而 `iAllowUnset` 只在「信箱哨兵／形狀可疑」那一支生效；另外兩支（persona 檔不存在、agent 欄為空）
是 `oProblems.Add(); continue;`，完全不看該旗標。

⇒ 讀的人照那行做會拿到**逐字相同的輸出**，下一個念頭多半是「我參數打錯了嗎」。
**壞的是指路，不是閘。**

### 實作形狀（SHA a7a8fb7，SCP_Core/master）
- 每則問題自己帶 `\n` 分隔的出口行；失敗分支只負責第一行掛 ⛔、其餘原樣印。
- 吃不下該旗標的兩支，加印定句「`allow_unset=1` 對這一支**無效**」—— 把假出口擋在門外。
- 只有信箱那一支印 `allow_unset=1`。

### 驗收判準（比「有沒有印」更硬的那一格）
⭐ **出口要能走得通，不是印得漂亮**：
`③ 不給 region ⇒ exit 3` → `④ 照出口補 region=Florin ⇒ exit 0`；
`⑤ 信箱形狀可疑 ⇒ exit 3` → `⑥ 照出口帶 allow_unset=1 ⇒ exit 0`。
⇒ 驗一條指路的唯一方法是**照它走一次**。

### ⚠ 沒做到的照實記
- ①② 的輸出仍逐字相同（那支本來就吃不下旗標）—— 治的是誤導，不是讓輸出分岔。
- ⑤⑥ 不是活體：現有 22 位 persona 沒有一位落在信箱哨兵那一支，用的是隔離 letters_root ＋ 刻意寫壞的 email.md。
