---
id: pitfall_prepared-key-drift
topic: streamwatch-cmd
title: 準備檔鍵漂移（TASK-0076）：一個欄位裝兩種東西＋fallback 把錯的擦成成功
type: pitfall
status: active
created_at: 2026-09-02
created_by: summit
links: []
related_docs: []
---

## 咬人的形狀（2026-09-01 七人同場實撞）

companion 的 `step=join` 拿到章 `0009` 而正解是 `0011` —— **兩條路都回 Success，差兩話**。
空著的 `0009` 會讓錯章號的心得看起來像「終於補寫了」，**沒有一格會紅**。

## 成因鏈（三層，每一層自己都是對的）

1. `session.media_id` 這個欄位裝的是 **start 收到的原字串**（可能是 work slug），欄位名說謊；
2. `ResolveWatchTarget` 遇到 **work 一對多**時（正確地）不自動選 ⇒ `library_media_id` **留空**；
3. 而每一個讀它的地方**都靜默退回原字串** ⇒ join 撈到 `prepared/<work_slug>.json` 舊檔。

⭐ **一對多從哪來（kiara 2026-09-01 量到）**：匯出實錄成書會在**同一個 work 底下**建出
`book-watch-<work>` 這個 media ⇒ **這隻專咬「已經被好好完成過、而且匯出過書」的作品，而那個一對多是永久的。**

## ⚠ 它的第三個下游（開單時沒人知道）

`library_media_id` 空掉 ⇒ **收工自動匯出走不了 `--from-session`**（它靠這個 id 讀準備檔）
⇒ 2026-09-01 第 11 話**當天沒有進實錄書**，六場的欄位全空，而整天沒有一格報錯。

## 修法為什麼長這樣（明天別把它改回去）

- **①煙霧偵測器**（`LoadPrepared` 對帳「檔名 ＝ 內容 media_id」，矛盾則拒用）與
  **②門鎖**（`session.prepared_key` 在 start 綁定、join 零 fallback）**是兩件事，別讓①去背②的責任**。
- ⛔ **不要改成「兩個檔名都找、找到就用」** —— 那是把撞名變成優先序問題，而**優先序錯的時候讀數仍然全綠**。
- ⛔ **不要自動修幽靈檔**（不改檔名、不改內容、不猜哪邊對）。
- `session.media_id` **不改名**（python 端 `library.py export-watch` 還在讀 `sessions_log.jsonl`）——
  已做的是**拆彈**：join 不再據它做任何決定。改名要兩端一起動，另掛一張。

## 追不到的那一格（別再花時間追）

落檔鍵從第一版 `577fe792` 到案發當天 `a400aff1` **一字未變**，全庫只有一個寫入端（python 端只讀不寫）
⇒ 現行與當時的 code **都生不出**那兩份幽靈檔。
而幽靈檔落地那一小時的樹**沒有進版控**（schema 從 10 欄長到 14 欄，最近的 commit 是五小時前）。
⇒ 是「**工作區狀態沒有被保存**」這一種追不到，不是沒查。**守衛不依賴成因，這正是第①刀的價值。**

## 側門（容易漏的一格）

`prepared/` 的讀取端有兩處：C#（`Cmd_StreamWatch.LoadPrepared`）與 **python（`library.py` 的
`--from-session` 直接讀檔）**。守衛只蓋 C# 那條時，側門的失效樣子跟「守衛沒掛」一模一樣。
⇒ 兩邊都要有同一條對帳。寫入端只有一處（C# `SavePrepared`）。
