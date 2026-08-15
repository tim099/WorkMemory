# 決策｜已讀水位 seq → 時間戳：整條收斂的遷移計畫

- **拍板人**：basecamp（Tim 2026-08-15 下放實作拍板權，tavern seq 15143；規格面才 @ Tim）
- **紅隊**：summit（判準見 tavern seq 15169；她明說自己不是核准人，只提砸磚判準）
- **狀態**：計畫已定，**code 一行未動**

---

## 1. 要解的問題（實測，不是推的）

已讀水位 `record.Seq` 是 **per-persona 單一 int**，而 seq 是 **per-room 編號**。
tavern 已到 15000+，側房 `trpg-yachiyo` 最大 109 ⇒ `CountInbox` 的 `seq > sinceSeq`
對側房**永遠 false**。那是**永久靜默，不是延遲**。

2026-08-15 全庫實測，六個在 state 內的 persona 全中，合計 **163 筆**永久看不見的 @：

| persona | 水位 | 遮蔽 | 房 |
|---|---|---|---|
| apex-one | 15031 | 39 | trpg-yachiyo 32 / trpg-midnight-relay 6 / story-whispering-grove 1 |
| summit | 15093 | 37 | trpg-yachiyo 34 / notify-mask-ab 3 |
| kaguya | 15044 | 33 | trpg-yachiyo 33 |
| basecamp | 15092 | 31 | trpg-yachiyo 25 / trpg-midnight-relay 4 / notify-mask-ab 2 |
| Sirius | 14998 | 15 | trpg-yachiyo 15 |
| meadow | 14494 | 8 | trpg-midnight-relay 8 |

> ⚠ 這是**病灶不是症狀**：`inbox` 早就是 per-room（`rooms/<room>/inbox/<persona>.md` 各一份），
> 是**水位還停在 per-persona 單值**。⇒ 這是「兩套水位不同源」，不是「seq 不夠細」。
> 改成 per-room dict 是把兩套都留著養大；**改時間戳是收斂成一套**。（summit 的講法比我準。）

---

## 2. 為什麼不能只改 `CountInbox`（本計畫最重要的一段）

`record.Seq` 不是一個判準，是 **12 個位置共用的水位**，而 `PendingSeq` / `CapMaxSeq`
同為 seq 語意**且與 `Seq` 互相比較**：

```
:363  HasPending => PendingSeq > Seq
:466  Seq = Math.Max(Seq, PendingSeq)
:476  CountInbox(..., record.Seq, ...)          ← 我原本以為的「唯一落點」
:495  HasNewEntryNewerThan(..., record.Seq, cutoff)
:508  Seq = maxSeq
:550  HasTimRemention(..., record.CapMaxSeq)
:572  CapMaxSeq = maxSeq
:715  HasInboxEntryInRange(..., Seq, PendingSeq)
```

只換 `:476` ⇒ **同一 record 裡兩套不同源水位** —— 正是本節開頭那隻病的新實例。

> ⛔ **而半套改法會通過驗收。** summit 的四步協議全部量在**讀取側**（水位被讀到了嗎），
> 而半套壞的是**推進側**（水位被推進了嗎）。差集恰好一筆、對照組 0 都會綠，
> 因為那條路徑真的通了 —— **綠燈亮，病灶已種。**
>
> ⇒ 教訓（summit 2026-08-15）：**判別器蓋不住你沒說出口的範圍。**
> 判別器的涵蓋上限不是設計者的功力，是**被告知的範圍**。她補的第五步（見 §5）
> 之所以能存在，是我先把範圍講對。

---

## 3. 改動範圍

| 現況 | 改成 | 備註 |
|---|---|---|
| `Seq`（int） | `SeqTs`（UTC ISO 毫秒） | **舊欄位保留並續寫**（見 §4 雙寫） |
| `PendingSeq` | `PendingTs` | `HasPending => PendingTs > SeqTs` |
| `CapMaxSeq` | `CapMaxTs` | `HasTimRemention` 改吃 ts |
| `CountInbox(sinceSeq)` | 吃 `sinceTs`，判 `entry.ts > sinceTs` | `seq` 降為診斷欄，**不刪** —— `UCL_RoomInboxStat` 的逐房分解要靠它讓遮蔽看得見 |
| `HasNewEntryNewerThan(sinceSeq, cutoff)` | 吃 `sinceTs` | 本來就在比時間，只是入口是 seq |
| `HasInboxEntryInRange(sinceSeq, uptoSeq)` | 吃 ts 區間 | 三信號之一，漏改會讓 pending 永不歸零 |

### 3b. 上表**不夠** —— 分類軸只有一條會漏掉三類（summit 2026-08-15 用不同切法對帳）

§3 那張表列的**全是「決策」落點**。她掃所有讀寫 `Seq` / `PendingSeq` / `CapMaxSeq` / `maxSeq`
的行，34 個命中、~15 個邏輯落點，多出來的**跟我列的不同種**：

**⛔ (i) 序列化預設值 —— 這隻會在第一次 tick 就炸，而且在所有判別器之前**

`:387` 現況 `Seq = kv.Value.GetInt("seq", 0)`。換成 ts 的自然寫法是 `GetString("seq_ts", "")`
⇒ **舊 state 檔沒有那個 key ⇒ `SeqTs = ""` ⇒ `entry.ts > ""` 對每一筆都成立 ⇒ 全部條目變成新的。**

⚠ **它不會等我跑 oracle**：daemon 每隔數秒自己 tick，domain reload 完第一拍就送出去了。
§5 的判別器全部在事後，**這一隻在事前**。

⇒ 需要的不是更好的預設值，是**明確的一次性 state 遷移步驟**（與 `inbox_ts_backfill.py` 同形：
dry-run 印出每個 persona 的 `Seq → SeqTs` → apply）。
且 fail-safe 方向必須是：**缺 `seq_ts` ⇒ 拒絕跑（或視為「已全部讀過」），不是視為全新。**
壞往安全的方向壞。

**⛔ (ii) 通知送出路徑的 `PendingSeq` 寫入點，不在那八行裡**

`:1197  record.PendingSeq = Math.Max(record.PendingSeq, chosen.MaxSeq);`（`RunOnce` 的 `if (notified)` 區塊）

漏它 ⇒ `PendingTs` 永不被寫 ⇒ `HasPending => PendingTs > SeqTs` **永遠 false**
⇒ 「戳了但沒證實讀到」整條線靜默失效。**長相是「一切正常，只是從來沒有 pending」。**

**⛔ (iii) 觀測層整片 —— 而那正是我要拿來查這次遷移的東西**

`:35` / `:76` / `:95-96` 三個類別各自帶 `int MaxSeq` / `AckedSeq`；
`:141` 人眼讀的那行「水位 seq {AckedSeq}／inbox 最大 {MaxSeq}」；
`:477-478` `trace.AckedSeq = record.Seq` / `trace.MaxSeq = maxSeq`；
`:540` 「水位 {record.Seq} 是別的房推上去的」；`:1269` 「快照 seq {r.PendingSeq}」。
**（＋ `:599 MaxSeq = maxSeq`，這個 summit 沒列，我自己掃到的。）**

決策走 ts、trace 仍報 seq ⇒ **我查問題時看到的數字，跟做決定的數字不是同一個量。**
那就是 `proxy-green`，而這次它長在**除錯工具**上 —— 會在我最需要看清楚的時刻騙我。
⇒ trace 欄位一起換，或並列兩個並**明確標示哪個是判準**。

> ⚠ 這三類不是我數錯，是**分類軸只有一條**：我沿著「哪裡做決定」掃，於是序列化、送出路徑、
> 觀測三條軸整片不在視野裡。⇒ **枚舉要換軸再數一次，不是同一軸數兩次。**
>
> ⚠⚠ **但這條規則的射程要標準確 —— 它只治一種病。** `:599` 是 summit 的枚舉也漏的那筆，
> 我原本歸因成「掉在她的軸外面」，**她自己查了原始 grep 輸出，第 28 行就是它** ——
> 不是沒掃到，是**掃到了、沒帶進清單**。
>
> | 失效 | 修法 |
> |---|---|
> | 掉在軸外面（basecamp） | **換軸再數一次** |
> | 掃到了沒帶過去（summit） | 換幾條軸都沒用 → **清單要從輸出生成，不要用手抄** |
>
> ⇒ 兩條規則各自標明射程，否則下一個人照第一條做，第二種還是會漏。
> （而這兩者是同一個母題的兩面：**「我驗過的東西」跟「我下結論的東西」中間差了一格** ——
> 2026-08-15 一天內第五個實例，前四個是 `LoadAllEntries` 只數一條路徑、漏數 C# 兩處、
> 互斥規則沒放一起看、行尾那個「量了位元組卻外推 git 後果」。**不是四種病，是同一格。**）

**前置條件（已滿足）**：每筆 inbox 條目都要有權威 ts。
- 寫入端：`AppendInbox` 已於 2026-08-15 補回 `_at <UTC 毫秒>_` 行（commit 待落）
- 既有條目：`inbox_ts_backfill.py --apply` 已回填 **681 筆**，回跑 907/907 有 `_at`（`a30292f0e`）

⚠ **順序約束**：`SeqTs` 初值必須在 backfill **之後**、從回填出的真 ts 取 max。
先設水位再 backfill ⇒ 水位是舊 seq 語意算的，而回填後的 ts 可能落在它之後
⇒ 那幾筆會冒出來（**殘缺版洗版，只漏幾筆、更難查**）。

---

## 4. 雙寫、單讀、舊欄位不刪（採納 summit ②）

同時寫 `Seq` 與 `SeqTs`，**只讀 `SeqTs`**。

- 這**不是**過渡期分支邏輯（那種東西「必須在某天被移除，而沒人記得移除」在本 repo 有血債），
  **它是資料** —— 不會製造「兩條路各自演化」。
- 而它讓回滾變成「**改回讀哪個欄位**」，不是「重建 state」。

⇒ 我接受這個區分：**我反對的是雙讀，不是雙寫。** 對稱不是理由。

---

## 5. 驗收（判別器在動手前釘死，不事後湊）

**A. 遷移 oracle（summit ①）** —— 新舊兩套邏輯各跑一次「這個 persona 現在會被通知哪些條目」取差集：

> 判準不是「有差異」，是 **差集恰好等於 §1 那 163 筆（＋遷移後新進的），一筆都不能多。**
> 多出來的每一筆都要能指名道姓解釋。

那張表是**動手之前**量的，所以有資格當判準（拿已發生的事實當判別器，不是事後湊）。

**B. 收件端協議（summit，收件端由她當）** —— 我不看 stdout / 退出碼 / 任何 md 投影：

1. 改完但還沒 @ → 通知她**重量基線**（05:10 那份**降為證物**，不當判準）
2. 我在 `trpg-yachiyo` @ 她一次，**只報送出時間，不報結果**
3. 她取差集：**恰好那一筆**（空＝紅；>1 筆也＝紅，那是水位沒設對、163 筆在湧）
4. **對照組**：同輪我再 @ 別人，她那側差集必須 **0**
5. **完整確認循環**（她補的第五步）：她讀那筆 → 隔時重量 → **同一筆不得再出現** → `HasPending` 回到無 pending

⚠ 第 2、3 步**不可同秒送** —— 水位是時間戳，同一秒兩筆會落在同一個比較點上，
於是分不出「對照組沒亮」是過濾正確還是被同一個水位一起吃掉。**兩件事同時發生就無法歸因。**

⚠ 3 防漏、4 防湧、5 防「推進側沒改」。缺任何一個，對應那類壞法都會拿到綠燈。

**C. 12 處由 summit 用不同切法獨立 grep 對帳（她 ④）** ——「12」目前只是我的枚舉。

---

## 6. 回滾路徑（summit ③ —— 寫進計畫不是寫在信心裡）

失敗長相是**通知行為變得無法解釋**：沒有例外、沒有紅燈、不會叫。所以要有五分鐘內可執行的退路：

**免編譯（真正的五分鐘止血 —— 只有這兩條）**
1. **關掉自動通知**（`UCL_BartenderAdminPage` 的「🔔 自動通知」，runtime + 永久兩顆）
3'. **換回 state 檔**：`remote_notify_state.json` 動手前存 `.bak`，直接覆蓋回去

**需要編譯（是「事後修好」，不是止血）**
2. **讀取端切回 `Seq`**（`Seq` 一路雙寫著 ⇒ 改的是一個欄位名，不是重建 state）
4. **git revert**（本層單獨一筆 commit）

> ⚠ **這個分組是 summit 砸出來的，而我原本把清單寫得比實際長**：我自己寫了「Editor 卡住時
> 編譯本身就不可靠」，卻把「切回 `Seq`」放進同一張退路清單 —— **那一步就是改 code + 編譯**。
> ⇒ 退路清單必須按「需不需要編譯」分組。**別讓退路看起來比實際多**，
> 因為需要它的那一刻，正是編譯最不可靠的那一刻。

---

## 7. 現況

- ✅ 寫入端已改（`AppendInbox` 吐 `_at`），`Recompile` **Errors 0**、快照晚於改動；
  **沒有任何 code 讀它 ⇒ 行為零改變**
- ✅ backfill 已落地：`f06ab1e`（工具）／`a30292f0e`（681 筆）
- ⏸ **讀取端一行未動** —— 等 summit 砸本計畫
- ⚠ 本計畫尚未經任何實跑驗證；§5 的三組判準**一組都還沒跑過**
