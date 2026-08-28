---
id: decision_submodule-page-decide-half-2026-08-28
topic: senate-backend
title: Submodule 頁補齊「決定」那半（逐項設定→編譯成指令）＋CLI --set-branch＋SCP_Ui.ToggleValue
type: decision
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/decision_git-layer-port-2026-08-26]
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/API/Cli_Reference.md, D:/Unity/Senate/Docs/Architecture/Ui_Framework.md]
---

UCL_GitSubmoduleSyncPage 移植第 4 步完成：Senate 端 Submodule 頁補齊「決定」那半，寫入鈕仍不放。

**拍板（詳見 Senate `Docs/Logs/Decisions.md` D19）**
- 分界線在**「決定」與「動手」之間**，不是「有沒有這個功能」。「寫入端不在頁面」（1fb3f4e 的拍板）約束的是誰動手，不是誰決定 —— UCL 原頁價值有一半在逐項設定，而那半完全唯讀。
- 頁面補上：逐顆納入/排除（Toggle）、逐顆目標 branch（Dropdown，選項含「(自動 → X)」）、fetch / include-root / push-all-remotes 三個開關；然後把意圖**編譯成可直接照抄的指令**（取代原本寫死的範本 —— 範本會在使用者改設定後靜默過期）。
- CLI 補 `--set-branch <path>=<branch>`（可重複）。沒有它，畫面上那個下拉就是「設了不會有事」的元件。守衛三格皆 exit 2：語法缺 `=` / 同路徑兩個分支 / 指到不存在的 submodule。
- 共用層補 `SCP_Ui.ToggleValue(id, fallback)`（讀勾選、不畫節點），刻意**不加** `SetToggle`。

**踩到的（都是實跑掉出來的）**
1. **掃描指紋沒有分隔符** ⇒ `(root="D:/A",branch="B")` 與 `(root="D:/AB",branch="")` 撞成同一個指紋 ⇒ 設定變了卻判成沒變 ⇒ 拿上一個 repo 的照片配新設定（＝ UCL 2026-08-11「綠燈全亮、量到的是別的 repo」的同形）。改用 `|` 當分隔符。
2. **fetch 開關與工具列「Fetch 後掃描」鈕搶同一件事**：開關進指紋之後，那顆鈕會在下一幀被「指紋說不帶 fetch」的重掃蓋掉 ⇒ 按了等於沒按且看不出被回滾。收斂成單一入口（鈕吃開關的值）。
3. **Fold 收合時子節點不建** ⇒ 收合那一輪讀不到 Toggle 回傳值 ⇒ 排除清單會被靜默丟掉，而丟掉後的畫面跟「本來就沒設」同形。這是 `ToggleValue` 存在的理由。連帶語意：收合時 `--toggle <id>` 會被「畫面上沒有這個 id」擋下（要先 `--fold` 展開）。
4. **我的工具參數裡的反斜線會被多折一層**：寫 `string.Join("", …)` 落地成 `string.Join("\x01", …)`（一個我沒寫的 0x01 控制字元），寫 `"\n"` 落地成真換行（編譯錯）。⇒ 寫 C# 字串常數時避開反斜線轉義，並在寫完後掃一遍控制字元（`ord(ch) < 0x20`）。編譯過不代表落地的是我寫的東西。
5. **`cmd | tail -5; echo $?` 量到的是 tail 的退出碼** —— 一個守衛的 exit 被我讀成 0（實際 2）。這條寫在 kiara 見林裡，這是第三次踩。

**驗收讀數（實跑）**
- `dotnet build Senate.slnx` 0 警告 0 錯誤；`selftest` 14 過 / 0 失敗 / 1 跳過（跳過那項是「讀真檔」缺樣本，與本次無關）
- 文字驅動逐格驗：Fold 展開、全排除守衛、收合仍生效的提示、選 master 後「來源」欄由 `啟發式` 變 `指定` 且指令長出 `--set-branch`
- **照抄畫面印的指令去跑** ⇒ exit 0、`目標=master（指定）`，與畫面一致（閉環）
- 截圖 `build/submodule_page.png` 視窗跑得動 ⇒ 沒有每幀重掃卡死

**留給下一個人的兩格（截圖暴露的既有版位缺口，刻意沒改）**
1. 三個 Toggle 的勾選框畫在文字右邊且彼此對不齊（`GuiImGuiRenderer` 對 Toggle 的既有畫法；本頁只是第一個一次放三個 Toggle 的頁面才讓它顯形）。UCL 那側 checkbox 在左。
2. 過長的 `Note` 在視窗裡被切掉右邊（不換行），同族於「表格還不吃 --width」。
⇒ 兩者都要動 renderer 層（UI 框架的決定），順手改會讓「這一頁的改動」跟「全站版位的改動」混在同一筆裡。

**還沒做的**：兩層改動未提交（SCP_Core 1 檔 + 主專案 2 檔 + 3 份文件），父層 gitlink 未 bump。Unity 端沒編過 `SCP_Ui.ToggleValue`（純新增 API、C# 9 相容，但沒有讀數）。
