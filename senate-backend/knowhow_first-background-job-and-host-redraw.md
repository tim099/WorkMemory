---
id: knowhow_first-background-job-and-host-redraw
topic: senate-backend
title: 本 repo 第一個背景工作：執行緒契約六條＋RedrawsContinuously＋兩段式確認要住 session
type: knowhow
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/decision_submodule-page-decide-half-2026-08-28]
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/Architecture/Ui_Framework.md, D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

Tim 2026-08-28 要求「加上 push & pull 按鈕，放在 TopBar」。詳見 Senate Docs/Logs/Decisions.md D19 ⑫。

**那條舊限制被正面解掉，不是繞開**
1fb3f4e 拍板「寫入端在 CLI 不在頁面」的理由（一輪批次分鐘級／純文字那側畫幾趟就結束 process ⇒ 丟背景等於什麼都不會發生／視窗那側同步跑會凍住）**仍然是對的** —— 它只是不再是「不做」的理由，而是「怎麼做」的規格。
⇒ `SubmoduleSyncJob`（Senate.Core，**本 repo 第一個背景工作**）＋ `SCP_GuiHost.RedrawsContinuously`（新宿主能力）讓同一份頁面碼在兩種宿主上都對：會重畫的丟背景並每幀顯示進度，不重畫的同步跑完才返回。

⚠ `RedrawsContinuously` **不是**「支不支援背景執行緒」（兩邊都支援），是「**背景工作跑完的時候還有人在看嗎**」。預設 false 是保守值：猜錯成 true 的症狀是「按了沒事」（跟「這顆鈕壞了」同形），猜錯成 false 只是「卡一下」（看得出來）。設定點在 Program.RunWindow（那一種宿主的性質），不在 Main。

**背景工作的執行緒契約（六條，沒有一條的違反是編譯錯誤）**
1. 背景只碰 job 物件（不碰頁面欄位/掃描結果/renderer 狀態）
2. UI 只透過 Snapshot() 讀（每幀拷一份，不持有內部集合引用）
3. 結果由 **UI 執行緒**搬進頁面（背景不直接寫頁面）—— 「誰擁有那份狀態」只能有一個答案
4. `Finished` 設在 **finally**：否則畫面永遠停在「執行中」，而操作鈕在執行中不畫 ⇒ **整頁鎖死**
5. 背景例外一定要 catch 並存起來：背景例外不會自己出現在任何地方，症狀是「進度停在 3/24」而畫面沒有錯誤
6. 進度計數是**估計值**，統計一律讀結構化的 Rows（靠字串前綴算統計會在有人改一個字的那天靜默偏掉）
⚠ **刻意沒有取消**：git 跑到一半被 kill 可能留下 index.lock 或半完成的 fetch，那個殘局比多等一會兒貴得多。每顆 git 自己有逾時（本機 120s / 網路 300s）。

**兩段式確認（不用 modal）**
寫遠端的兩顆按一次變成「⚠ 確定執行…」＋「取消」，再按才跑 —— 跟 CLI 端 --push 要求 --yes 是同一道手勢。用兩段式而非 modal：共用層沒有 modal 節點，兩段式用既有節點組出來 ⇒ 四種驅動天生就會、CLI 可重放。

🩸 **待確認態第一版放頁面欄位 ⇒ 跨 process 就丟了**（按 run-push 之後 --list 列出來還是三顆操作鈕，純文字那側永遠停在第一步）。改住 session（submodule/pending）。
📌 **這個坑我今天已經踩過一次**（生效值 root/branch），同一天用同一個形狀又踩第二次 ⇒ 判準還沒內化：**任何「按了之後要記住的東西」在 CLI 宿主都必須住 session。**

🩸 **工具列的鈕差點永遠不出現**
第一版在工具列寫了「m_Scan 沒資料就不畫鈕」的閘。而**工具列先於內容區畫**、掃描在內容區 ⇒ 工具列看到的 m_Scan 永遠是 null。視窗那側第二幀就有（看不出問題），而**純文字那側只畫一趟 ⇒ 完全看不到操作鈕**（實測 ui --list 只列得出「重新掃描」）。
⇒ 鈕無條件畫，按下那一刻才 EnsureScannedForJob。而那個確保要**分兩階段**：RunBatch 用的目標 branch 來自照片的 TargetBranch ⇒ 一張「不帶 overrides」的照片會讓逐項覆寫**靜默失效**（照啟發式目標去切而報告一排 ✓）。

**Q0：畫面文案與檔頭在說謊**
「要動手的話（寫入端在 CLI，不在這一頁）」—— 寫入端現在就在這一頁。同族還有檔頭「這一頁刻意不放寫入鈕」與 Cli_Reference 的「頁面唯讀」。三處都改，並把「它曾經為什麼成立」留下來。⚠ 第一處是我從自己的截圖上看到的。

**Q0：工作區行尾混成一團（值得記的方法論）**
`Write` 工具建的檔是純 LF、`Edit` 改的保持 CRLF，而我的 python 插入接字串時用了 LF ⇒ md 變混合。
📌 判準：**`git diff --ignore-cr-at-eol --stat` 是分辨「真改動」與「行尾假 diff」的那把尺，而 `git status` 的 M 兩者長得一模一樣。**
量出來的：Ui_Framework/Decisions 統一成 CRLF 後**剛好等於 index**（＝修掉我自己造成的混合）；Cli_Reference 有 32 行純行尾 diff（＝**我把混合行尾 commit 進去了**）。

🩸 **反斜線第三次咬我**：在 heredoc+python 裡寫「\n」會被多折一層，落地成**真換行**（把 Decisions 的一句話斷在中間）。前兩次是 `string.Join("", ..)` 落地成 0x01、`"\n"` 落地成真換行。
⇒ **在 heredoc + python 裡完全避開反斜線**（改寫措辭，或用 chr(92)）。並且**寫完一定要掃行尾與控制字元**。

**驗收（每一步都是獨立 process）**
工具列列得出 run-pull/run-push/run-sync；截圖確認六顆鈕排同一行沒溢出；按 run-pull ⇒ `✓0 ⏭1 ✗0`、「沒有做完的」列出 `SCP_Core ⏭ dirty`（安全線生效，**沒動任何東西**），跑完自動重掃 ⇒ 出現 `↑1 ↓0`；按 run-push ⇒ 換成「⚠ 確定執行「push」」＋「取消」，**新 process 那個確認態還在**，按取消回三顆鈕，SCP_Core 全程 ahead 1 沒被推。build 0/0；selftest 14 過、--clipboard 16 過；九個改動檔行尾統一 CRLF、零隱形字元。

**沒有讀數的一格**：視窗模式的背景執行（不凍結、進度每幀更新）要人開窗按一次 —— 截圖模式沒有點擊入口，8 幀也拍不到分鐘級工作的中段。**不宣稱它被驗過。**
