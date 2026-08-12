---
id: state_2026-08-13-broadcast-timeout-and-docs
topic: awakening-flow-rework
title: 廣播 timeout 對稱補齊 + 早安文件三件（未 commit）
type: state
status: active
created_at: 2026-08-13
created_by: summit
links: []
related_docs: []
---

2026-08-13 收工快照（summit wake#46）。

## 今日落地（UCL_Core，未 commit）
- `awakening.py`：新增 `BROADCAST_TIMEOUT_SEC = 30.0`，套上 morning / intro / rest / relogin
  四個 `tavern_post` 呼叫點；goodnight 維持 12s（Tim 2026-07-22 拍過的值，未動）。
  ⇒ 五個 ritual 廣播現在全部顯式帶上限。
- `ucl-morning/SKILL.md`：② 路徑改「以 morning 印出來的那行為準」＋ `_lib/ucl_paths.py` 一行；
  新增「🚪 卡住了怎麼辦」（`brief` / `reissue-token` / `relogin`，只補工具訊息沒說的三條）。
- `reading-library/SKILL.md`：letters 根不寫死（override key + pointer 兩條漂移途徑點名）。
- `Awakening_Ritual_Workflow.md`：新增「⏱ 落檔順序與殘餘窗口」＋「📣 廣播的等待上限」＋ Template 用法；
  修掉兩處過期資訊（「morning 末尾自動重生成」、`persona_registry.json` 單檔）。
- `Plan_Awakening_Flow_Simplification.md` §8：Cmd 化 + `next` 導引的**備忘**（Tim 明示先不遷移）。

## 驗收方式
`Template` 測試殼（不拿真人 persona 當白老鼠）：
`morning --persona Template --agent ClaudeCode --model test` → EXIT=0 / 4s /
`🧠 wake brief 落檔…（先於上線廣播）` 出現在 `tavern_post: OK` 之前。

## Pending
- 全部未 commit（UCL_Core 單層 + AgentCommands + 主專案指標）。
- Cmd 化提案未拍板（見 plan §8.7 三個未決）。
- **218 秒窗口的去向仍未知** —— 我原本的「卡在 tavern_post」推論已被自己推翻（見 pitfall）。
