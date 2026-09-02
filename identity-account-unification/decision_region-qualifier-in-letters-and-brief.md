---
id: decision_region-qualifier-in-letters-and-brief
topic: identity-account-unification
title: 地理定語：收尾信寫 region/project、brief 印現地（Tim 2026-09-02 拍板）
type: decision
status: active
created_at: 2026-09-02
created_by: basecamp
links: []
related_docs: []
---

**Tim 拍板（2026-09-02）**：用 Bank 的區域（貨幣）ID 當「現在站在哪一區」的定語，
落點兩處 —— 晚安信自動記錄（寫入端）＋早安 brief 看得到（讀取端）。
不同 repo 用不同 `currency_id`，一對一**由慣例維持**（目前只有兩個 repo）；
我提的「另造 canvas_id」被拍掉，我收回。

## 實作（都已落地並回讀驗過）

- **寫入端**：`UCL_AwakeningService.WriteWakeLetter` 的 machine frontmatter 加
  `region`（＝`Treasury.UCL_CentralBankSettings.CurrencyId`）與
  `project`（新增 `ProjectNameOfDataRoot()`＝資料根上一層目錄名，取不到回 `unstated`）。
- **讀取端**：`SCP_WakeBrief.Build/Write` 加 **optional** `iRegion`；frontmatter 兩欄＋標頭「📍 現地」段；
  新增 `ProjectOf(iDataRoot)`（純路徑運算）。`SCP_Cmd_WakeBrief` 加 ArgSpec `region` 透傳。
- **宿主接線**：`UCL_AwakeningService.RunBrief` 傳 `CurrencyId` 進去。

## 為什麼是兩個欄位（這格是設計的核心，別簡化掉）

`UCL_CentralBankSettings.CurrencyId` **缺值時回預設 `Ducat` 而不是空** ⇒
兩個沒設定過 `currency_id` 的專案會印出**同一個 region**，而那正是這個定語要防的對撞。
`project` 在那種情況下仍然分岔 —— **恆同的欄位不帶資訊**（同一份 code 的 registry 對帳段
就是因為兩邊變同源而退化成裝飾）。⇒ region 不給時印 `unstated` 並明說宿主沒給，**不補預設**。

## 射程（寫進輸出裡，不只寫在這裡）

會隨 data_root 分岔的只有兩條軸：**2D 畫布座標**（`Canvas/` 是一般目錄）與
**酒館 seq**（`ChatTavern/` 是一般目錄）。
`Sculpture/`、`Chess/`、`Tasks/`、`BookNotes/`、各 persona 信件庫**都是 submodule** ⇒ 單一全域軸。
⛔ 給不會分岔的東西加定語＝宣告它會分岔（有人會去找另一區的 TASK-0100，而那不存在）。

## ⚠ 未驗／未生效（接手的人先看這兩格）

1. **CLI 原生那條還沒生效**：`SCP_Core` 是同一個 repo 掛兩份
   （`Bar/Assets/Plugins/SCP_Core` 與 `Senate/SCP_Core`，比過 HEAD 都是 `3bf913c`）。
   我改的是 Bar 那份工作副本 ⇒ `senate cmd wake-brief` 要等 Senate 那份 pull 才有 `region` 參數。
   **Editor 這條（Tim 早上實際走的）現在就對。**
2. **舊信不回填**：磁碟上沒有舊信的區域資訊，補出來的是編的。舊信無此欄＝**未宣告**，
   ⛔ 讀取端不准腦補成「就是本區」（採 @kiara 的版本，不採「default: BTC」）。
   而 brief 印的是活值 ⇒ 讀取端**零 legacy 處理碼**，只要文件寫一句基準日。

## 驗收讀數（Tim 指定用 Template persona）

- `Template/wakes/000004`：`region: BTC` / `project: Bar` 落在紙上。
- `Template/wakes/000005`：**對撞測試** —— 信裡故意自寫 `region: FAKE_REGION`，
  結果機器值勝出、作者版留痕 `region_as_written`、非機器欄位 `mood` 保留
  ⇒ 「不讓 agent 親筆」由機制擋住，不靠自律。
- 讀取端走生產路徑（`senate cmd morning-brief` → `RunBrief`）：我自己的 brief 檔頭出現兩欄，主檔 1151 行。
- 編譯重跑過：`20:59:21 / 7.712s / errors=0`（warnings 21→67 全是 SCP_Core assembly 首次重編的 CS8632，我新增最多 4 筆）。
