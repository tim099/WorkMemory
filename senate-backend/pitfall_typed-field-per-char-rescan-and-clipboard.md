---
id: pitfall_typed-field-per-char-rescan-and-clipboard
topic: senate-backend
title: 打字欄位逐字元重掃＋生效值跨 process 丟失＋ImGui 吃不到 Ctrl+V（全站）
type: pitfall
status: active
created_at: 2026-08-28
created_by: kiara
links: [senate-backend/decision_submodule-page-decide-half-2026-08-28]
related_docs: [D:/Unity/Senate/Docs/Logs/Decisions.md, D:/Unity/Senate/Docs/Architecture/Ui_Framework.md, D:/Unity/Senate/Docs/API/Cli_Reference.md]
---

Tim 回報三件事，兩件是我種的坑。詳見 Senate Docs/Logs/Decisions.md D19 的 ⑧ ⑨。

**① 🩸 打字欄位「值一變就重掃」= 逐字元跑 git**
在視窗裡打字是**逐字元事件**，所以打 `D:/Unity/LY` 會觸發 11 次重掃，每次跑一整輪 git（LY 有 24 顆 submodule）。我上一輪只防了「每幀重掃」（指紋），完全沒想到「每字元重掃」。症狀不是慢，是視窗在打字期間卡死，而它看起來像「這個欄位壞了」。
⇒ **打字類走草稿＋套用鈕；點選類（勾選/下拉/改回自己/挑專案）維持立即生效**（單次離散事件，而且立即看到結果才是那些元件的價值）。
配套：「放棄改動」把生效值寫回欄位；草稿≠生效值時印一行「下面的表格與指令仍然是 X」；表格與指令一律吃生效值不吃草稿。

**② 🩸 生效值只放頁面欄位 ⇒ 跨 process 丟失**
CLI 每次呼叫都是新 process，頁面實例重建、m_Scan 是 null ⇒「我上一步套用的是 LY」消失，每個指令都得先按一次套用。抓到它的讀數：`--set submodule/root=X` 之後按「放棄改動」，欄位回到 Senate 而不是剛套用的 LY。
⇒ 生效值住 session（`submodule/root/applied` / `submodule/default-branch/applied`，跟 Dropdown 的 `<key>/value` 同一個模式）。
📌 一般形：**「現在生效的是什麼」跟「使用者正在編輯什麼」是兩份狀態，而前者的生命週期必須跟宿主的生命週期一樣長。**
連帶把 OnPush 的掃描拿掉：要掃誰取決於 session 生效值，而 OnPush 沒有 SCP_Ui 可讀 ⇒ 在那裡掃只能掃 Senate 自己，然後 DrawContent 用生效值再掃一次 = 每次開頁白跑一輪 git。掃描收斂成一個落點（指紋比對，m_Scan==null 也算不符）。

**③ ImGui 的 InputText 吃不到 Ctrl+V —— 全站問題，不是這一頁**
查證：`SetClipboardTextFn` / `GetClipboardTextFn` 全 repo 零命中，Silk.NET 的 ImGuiController 不會自己設 ⇒ **視窗模式下每一個輸入框都貼不上**。
短路徑：`SCP_GuiHost.ReadClipboard`（新宿主能力，Windows 繞 PowerShell Get-Clipboard -Raw）＋ 一顆「📋 貼上」鈕（沒掛實作就不畫）。回傳三格 Ok/Text/Message（「剪貼簿是空的」與「讀不到剪貼簿」不得同形）。貼上只填草稿不套用；剝掉包住整串的雙引號（檔案總管「複製路徑」帶引號 ⇒ 直接送 Directory.Exists 一定 false，而畫面說「路徑不存在」，使用者會以為自己複製錯了）。只掛按鈕不放每幀路徑（PowerShell 啟動約半秒）。
⚠ **這是繞道不是修好**：使用者肌肉記憶是 Ctrl+V，而且問題涵蓋全站。正解是接 ImGui callback（Marshal.GetFunctionPointerForDelegate ＋ 自己管一塊 UTF-8 buffer ＋ Win32 OpenClipboard/GetClipboardData，約 60 行 unsafe 在 Senate.Desktop）。刻意不順手做：寫壞的症狀是整個視窗掛掉，該單獨做單獨驗。

**驗收（每一步都是獨立 process，所以順便驗了跨 process）**
預設 Senate（1 顆）→ set root=LY 不重掃且印「還沒生效」→ apply ⇒ 24 顆 → 新 process 不帶動作仍是 LY 24 顆 → 設假路徑後 discard ⇒ 欄位回 LY → root/self ⇒ 1 顆且無「還沒生效」。
branch 同樣走草稿：set default-branch=Dev 印「還沒生效」，套用後 SCP_Core 那列變 `目標 Dev / 來源 全域預設`。
貼上：剪貼簿放 D:/Unity/LY ⇒ 欄位填入、訊息「讀到 11 個字元」、表格沒動；放 "D:/Unity/LY"（帶引號）⇒ 讀到 13 字元而欄位是 11 字元乾淨路徑。
build 0/0；selftest 14 過 0 失敗 1 跳過；五個改動檔零隱形字元；收工 ui --reset 後 session 裡 submodule 相關 Fields/Toggles 全歸零。

**留給下一個人**
1. Ctrl+V 全站支援（見③的正解）—— 這是最該做的下一格
2. 上一輪那兩個既有版位缺口還在：三個 Toggle 勾選框畫在文字右邊且對不齊、過長 Note 被切掉右邊
3. 兩層改動未提交（SCP_Core 2 檔 + 主專案 3 code 檔 + 3 份文件），父層 gitlink 未 bump
