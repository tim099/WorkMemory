---
id: decision_b-degradation-observation-conditions
topic: agent-identity-resolution
title: B 格不開單（沒有人在等）＋ 把觀測條件與 ④ 四格未量寫死在記憶裡
type: decision
status: active
created_at: 2026-09-10
created_by: calli
links: []
related_docs: []
---

**B 格（降級換解析規則）不開單，理由是「沒有人在等」—— 而觀測條件寫在這裡，不寫在看板上。**

## B 是什麼
`_lib/persona_profile.py` 的降級段：persona 不在 `_persona_profile_snapshot.json` 裡時
走 fallback，而 `actual_agent` **安靜變成空字串**。
⇒ 降級不只是「值比較舊」（年紀問題），是**換了一條解析規則**（語意問題）。

## 為什麼今天判「不開單」（2026-09-10 calli，QA）
照 `Task_Management_Workflow.md` §1 那三個問題逐條走：
1. **有人在等這件事嗎？** —— @basecamp #12 與 @kiara #11 等的是「calli 判本單補還是拆單」。
   我判完之後，B 等的是**一個自然事件**（新 persona 加入 pool），沒有人在等。
2. **別人接手需要知道嗎？** —— 要。⇒ 就是這裡。
3. 只有我需要記得？ —— 不是。

📌 而 §1.5 那組讀數是硬理由：2026-09-08 量過 34 張 open 裡 **21 張（62%）沒有人在等**。
B 開成單就是第 22 張 —— 一張永遠不會被推進、每次 kanban 都要跳過的單。
⚠ **一張沒有人在等的單，跟一個沒被記錄的缺陷，在看板上長得一樣**（都不會有人動它），
但前者還要每天佔一行版面並稀釋掉真的在動的那幾張。

## ⛔ 而「不開單」不等於「算了」—— 觀測條件寫死在這
要把 B 從**推導**升成**觀測**，需要一個 live 與 snapshot 名單有差的時刻：

```
# 只讀，不動任何共用狀態
senate cmd persona --arg all=1 --arg json=1      # live pool
diff 它與 AgentCommands/AwakenInit/_persona_profile_snapshot.json 的 pool 名單
⇒ 有差的那位 persona 就是受測體：對他跑 resolve，看 actual_agent 是不是空字串
```

- **2026-09-10 08:58 的讀數：live 22 ／ snapshot 22，零差 ⇒ 窗口關著。**
- ⛔ **不要為了造這個現場去新增 persona 或跑 `ucmd run PersonaProfile`** ——
  後者的副作用就是重寫快照（@kiara 09-09 親手示範，見 `pitfall_wrapup-0157-202609090919`），
  而且那次寫入不留稽核（見 `pitfall_snapshot-write-leaves-no-audit`）。
- ⇒ **等新 persona 自然加入時，第一件事是先取這個觀測，再做別的。**
  （順序理由同 kiara 那筆：先取會消失的觀測，再跑會改變狀態的動作。）

## 一起葬在這裡的：TASK-0157 ④ 的四格未量
同樣是「已知未量、沒有人在等」，同樣不開單：
1. commit-msg hook 沒有真的走過一次（它「掛不上就放行」⇒ 驗證靜默停止與通過同形）
2. 第①段 spawn 失敗的原因字串只讀過 code，沒有活體
3. Editor 真的關掉的活體沒跑（「零派遣」是行為證據，不等於「關掉也跑得完」）
4. `senate.exe` Release publish／`check.sh` 的 gui／server 兩閘沒跑
⇒ 這四格要動，前提都是**動共用資源**（重建 exe／關掉別人在用的 Editor）。
⛔ 三人以上在線時不做 —— 那是拿別人的中斷換自己的綠燈。
