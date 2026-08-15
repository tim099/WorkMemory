---
id: state_2026-08-12_morning-hardening
topic: awakening-flow-rework
title: 2026-08-12 早安流程加固：line_buffering + brief 前移 + Template 測試殼
type: state
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

**今日（2026-08-12）ship 兩塊，皆已 commit 或待 commit；本條是 state 快照。**

## ① awakening.py 三處修改（UCL_Core，**尚未 commit**，Tim stage 的是另一層）

1. **`line_buffering=True`（:84-95，stdout 與 stderr 都開）** —— pipe 下 python 預設整塊緩衝，A/B 實測第一行 `3.03s → 0.06s`，總時長不變。⚠ **它治「看不見進度」不治「跑太久」**，超過呼叫端 timeout 照樣被背景化。stderr 一併開是我加的（原案三方都只點名 stdout，但 morning 的警告全走 stderr）。
2. **`write_wake_brief_files()` 抽出 + Step 4.5 前移**（brief 落檔排到 `tavern_post` 之前）—— 「有記憶才准宣告在線」。**已由 summit 跑 Template 全流程拿到生產驗收**（`🧠 wake brief 落檔` 出現在 `tavern_post: OK` 之前）。
3. **殘餘窗口註解**（`write_lock` 仍先於 brief）：exception path 有 stderr 會叫；**kill path 進程死了什麼都不印**。B 案（lock 記 `brief_written`）**未實作**。

## ② Template 測試殼（AgentCommands，**已 commit `943172b9`**）

persona 檔 + 十層範本資料（憲法／見根／見叢／見森／見林／見樹／畫像／速寫／書架／掛號信 mailbox+outbox／rests／dialogues／README）。brief 104 → 271 行。

## 📍 pending（等 Tim 拍板，一行 code 都沒動）

- **產線收斂**：C# `DoCreatePersona()` 與 python `fork_persona()` **是兩份實作、今天恰好欄位一致**。建議 python 當唯一產線、後台走 Cmd。
- ~~**`kind` / `is_synthetic` 旗標不存在**~~ → **已關閉（Tim 2026-08-15 拍板，非實作關閉）**：帳戶操作綁 `persona` ⇒ 系統訊息本來就不動帳；且 Template **必須走一樣的流程，帳戶本身也是測試目標之一**。⇒ 旗標**不需要做**。⚠ 這條原本的敘述把「Template 不該動帳」當既定事實，**那個前提從未被拍板，是 basecamp 自己補的**。
- ~~**四個 `tavern_post` 呼叫點補 timeout**~~ → **已完成**（2026-08-12 起 ritual 五個呼叫點全部顯式帶值：goodnight 12s、morning/intro/rest/relogin 30s，見 `awakening.py` `tavern_post` docstring）。
- **B 案**（kill path 磁碟證據）—— kill path 至今**零現場血證**（apex-one 親口證實 wake#23 在背景跑完且生了 brief），**不做的理由已從「沒拍板」升級成「沒有證據要求它存在」**。
- skill / 文件三件在 summit 手上（`ucl-morning` 路徑改引 `ucl_paths.py`、出口段、`reading-library:67` 同族寫死路徑）。
