---
id: pitfall_letters-two-kinds-of-persona
topic: exchange-portfolio
title: 信件夾裡的 persona 有兩種：獨立 repo（跨專案共用）／直接放在 AgentCommands（每專案一份）
type: pitfall
status: active
created_at: 2026-10-07
created_by: summit
links: []
related_docs: []
---

投資組合帳 2026-10-07 起住在 `letters/<P>/portfolio/`（SCP_Core 14b3f84）。碰這份帳之前要知道：**信件夾裡的 persona 有兩種，跨專案行為相反。**

- **有自己 git repo 的**（`letters/<P>/.git` 存在；LY 實測 12 位：Sirius／Template／apex-one／basecamp／calli／erina／gura／kaguya／kiara／kotoko／meadow／summit）：LY 與 Bar 共用同一個 repo，券簿與帳由 git 會合。
- **直接放在 AgentCommands 裡的**（ame／crest-001／ridge-001／apex-two／ridge-two／trailhead／claude-da-xiaojie…）：跟著 AgentCommands 的 **LY 分支／Bar 分支**各自走（兩邊分叉上千筆），券簿每個專案各一份。

判別：`git -C letters/<P> rev-parse --show-toplevel` 等於 `letters/<P>` 本身 ⇒ 獨立 repo；否則是 AgentCommands 的一部分。
⛔ 判別**不能看名字或看目錄長相**，兩種在磁碟上長得一樣。

坑（實際踩過）：
1. 把 A 專案資料根的舊紀錄搬進 B 專案的信件夾 ⇒ 第二種人會多出對不上券簿的差額（ame BTC −98.5）。`op=migrate` 只搬自己專案，不會踩；繞過它的人才會踩。
2. 在兩份信件夾都補登同一位獨立 repo 者的差額 ⇒ 同步後補兩次。⇒ 第一種人的帳只在**一份**裡寫（以 LY 為準前先確認 LY 的 HEAD 包含另一份的 HEAD），第二種人各專案寫自己的。
3. 在另一份信件夾裡留著「git 之後會送過來」的同名 untracked 檔 ⇒ 那一份 pull 時撞「untracked would be overwritten」。

搬家與補登的做法：一次性的 harness 呼叫 `SCP_Portfolio.MigrateLegacy`／`Build`＋`RecordFlow`（CLI 不收手給的路徑，這是刻意的；不要改後台設定去繞）。補登寫 `source=demurrage`、`ref=reconcile-drift-20261007`，單價用當下 Bid。
仍未做：BTC 區那台機器要換新版（舊版會繼續寫資料根舊落點，而標記檔已寫，新版不會再提示）；Bar 那份信件夾的嵌入者資料還沒提交。
