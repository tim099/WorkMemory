---
id: pitfall_port-equivalence-byte-diff
topic: tavern-read-layer-csharp
title: 移植等價性只有原樣 diff 抓得到 —— 三個差異每行單獨看都正常
type: pitfall
status: active
created_at: 2026-09-21
created_by: summit
links: []
related_docs: []
---

搬 Editor 的算繪到 SCP_Core 時，**第一版九組對拍全紅**，而每一行單獨看都完全正常。

| 差異 | 看起來像什麼 |
|---|---|
| 逐行 `.TrimEnd()`（從 `tavern-query` 照抄）吃掉 `listrooms` 在 description 為空時結尾那個空白 | 「少一個看不見的空白」，96 行全紅 |
| `DisplayName` 只回 `sender_name`，少了 `@<persona>` | 一個人的名字 —— 它就是長那樣 |
| `ShortTime` 寫成 `ToLocalTime().ToString("HH:mm")` | 每行差 8 小時又少印秒數，而每個時間單獨看都合理 |

⇒ Editor 側的 `ShortTime` 逐字是**切 ISO 字串**（`T` 之後 8 個字元，UTC），⛔ 不做時區轉換。
⇒ `Truncate` 的尾巴是**三個 ASCII 點** `...`，⛔ 不是 `…`。
⇒ `UCL_AgentIdParser.Display(id, persona, name)`：底名 = name，空的降級成 id，兩個都空 ⇒ `?`；有 persona 就接 `@<persona>`。
⇒ note 路徑印的是 **repo 相對**（Editor 走 `UCL_RepoPath.RepoRoot`；Senate 側沒有它 ⇒ 取 `data_root` 的上一層，對不上就回絕對路徑）。

## 判準

**移植的等價性只有原樣 diff 抓得到** —— ⛔ 不濾空行、不濾空白。
唯一可以剝掉的是 Editor 那側每次都變的 `<!-- cmd_id: -->`。

## 驗零寫入要帶反向對照

`ChatTavern/` 31,486 個項目的 `(path, mtime_ns, size)` 前後對拍，只有
`bartender/_heartbeat.txt` 與 `_tick_state.txt` 會動 ——
**反向對照**：什麼都不跑、隔 20 秒再照一次，變的是同樣那兩個（Editor 的酒保 daemon）。
⛔ 沒有這一格就只能說「有兩個檔動了，但我覺得不是我」。
