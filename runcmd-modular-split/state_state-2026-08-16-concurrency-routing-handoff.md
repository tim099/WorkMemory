---
id: state_state-2026-08-16-concurrency-routing-handoff
topic: runcmd-modular-split
title: queue 自動路由開了又關；併發前置未補，交接 basecamp
type: state
status: active
created_at: 2026-08-16
created_by: summit
links: []
related_docs: [commit:5325d18, commit:38a40be, ucl_core:Docs~/zh-Hant/Plan/Plan_Cmd_Concurrency_Hardening.md]
---

**交接對象：@basecamp（Tim 2026-08-16 指派）。本案後續拍板權與所有權移轉，我不再自行動 code。**

## 一、我今晚動了什麼（兩筆，第二筆是把第一筆關回去）

| SHA | 內容 | 現況 |
|---|---|---|
| `5325d18` | ① `--arg persona=` 自動路由到 `queues/<persona>/` ② stale-read 標記 ③ ack timeout 60→180 | ① 已被下一筆關閉；②③ 仍生效 |
| `38a40be` | 新增 `AUTO_ROUTE_BY_ARG_PERSONA = False`，把 ① 關回去 | **行為＝5325d18 之前**（實跑確認落回 `anonymous`） |

⇒ **現在 repo 的行為與今晚開工前一致**，除了兩個純加法（stale 提示、等待上限）。

## 二、我為什麼要關掉它（給接手的人的判準，不是道歉）

我開 ① 時對 Tim 說「C# 不必改，我查證過」。**查證只做了一半**：
我讀到 `ListAgentIds()` 列舉資料夾、watcher 對每個 id 輪詢 ⇒ 結論「路由得到」，
**沒有問下一句：那些 id 是輪流跑還是同時跑。**

補讀之後（自己讀的，不是引用妳的 plan）：
- `UCL_AgentCommandWatcher.OnEditorUpdate`：`foreach (agentId in ListAgentIds()) TryDispatchAgent(agentId);` —— **不等前一個完成**
- `UCL_AgentCommandRunner` 重入閘 `s_RunningAgents` 是 **per-agent**：同 agent 擋、**不同 agent 併行**

⇒ **不同 queue 資料夾會真的同時跑。** 而妳 plan §0 的「當日修正」框已經寫死了正確順序：
**§4（per-cmd 回傳槽）先於 §2（路由）**。**我沒讀那份 plan 就動手，原樣復現了妳推翻掉的版本。**

## 三、code 現況（接手直接看這幾處）

`run_cmd.py`：
- `AUTO_ROUTE_BY_ARG_PERSONA = False`（`ANONYMOUS_QUEUE_ID` 下方）—— **開啟只要改這一行**，
  上方註解寫了併行事實、五個全域槽清單、前置條件
- `_persona_from_cmd_args(args)` —— 已寫好且測過，**保留**。刻意不吃 `--arg-stdin`（stdin 只能讀一次，
  在那裡讀掉會讓真正的 arg 解析拿到空 body，且是靜默的）；刻意不做大小寫正規化（否則會出現
  「args 裡是 Summit、queue 落在 summit」的分岔，而那種不一致沒有一格會叫）
- 路由對拍七例我跑過：顯式優先／空值不路由／`persona=../../evil` 被既有防護擋回 anonymous
- `_warn_stale_payload(args)` 掛在逾時出口；`DEFAULT_ACK_TIMEOUT = 180.0`

## 四、我驗過的 vs 我沒驗的（分開列，別混）

**驗過（自己讀／自己跑）**
- watcher 逐 agent 派遣、`s_RunningAgents` 是 per-agent → 併行為真
- 路由七例對拍；只帶 `--arg persona=summit` 實跑落 `queues/summit/` 並 Success
- 關閉後實跑落回 `anonymous`
- stale 提示實測輸出，正確指到今晚騙過我的 `_streamwatch_cycle.md` + mtime
- submit 順序是 `ensure_idle → 寫 queue.json → 寫 trigger`（資料夾在 trigger 前就有 queue.json）

**沒驗（別當已知）**
- ⛔ **「兩人同時撞 lane」的真實情境沒重現** —— 今晚那條件要多 persona 同場才長得出來
- ⛔ **併行下五個全域槽會怎麼串，我沒做過任何實驗** —— 我是從 code 推的，不是量的
- ⛔ ②③ 只在單人情境下跑過

## 五、Tim 今晚新丟進來的一格（妳 plan 裡目前沒有）

> **`--agent-id` 沒有唯一性保證；唯一有守衛的是 persona（不得同時登入兩次）。**

佐證：`UCL_AgentCommandsPage.cs:168` 自己寫「agentId 是 caller 自由填的字串」。
⇒ 現行優先序 `--agent-id > --persona` 在**併行打開之後**是第二個坑：
兩個實體填同一個 id 就共用 lane，`ensure_idle` 互等、全域槽互串。
⚠ 我的判斷是**現在不要改它**（在關著的門上換鎖），但這格要進妳的 §3 清單。

## 六、我認為最先咬人的那一格（給妳排序參考，不是結論）

`Cmd_Tavern.cs:513` 的 `public static int LastPostSeq`，因為 `Cmd_StreamWatch` 的用法是
`LastPostSeq = 0; await Cmd_Tavern.Execute(...); return LastPostSeq;`
—— **寫入與讀取之間隔著一個 `await`**。併行時會拿到別人的 seq，
**而 seq 是完全合法的數字，回傳檔照印，沒有任何一格會紅。**

其餘四個：`s_CurrentCmdOutputs` / `s_CurrentCmdValues` / `CurrentCmdId` /
`UCL_TreasuryLedger.CurrentCallerEnvMarker`（最後這個是**記帳來源標記**，串線會影響錢）。

## 七、我對接手的兩個請求

1. **別把「summit 已經測過路由」當成路由可以開** —— 我測的是「落到對的資料夾」，
   不是「併行下值不會串」。那兩件事今天被我混成一件過一次了。
2. 翻 `AUTO_ROUTE_BY_ARG_PERSONA` 那一行時，**要有第二個人驗** ——
   照今天的規矩：宣稱的人不能是唯一的證人。
