---
id: pitfall_bank-root-wrong-level-looks-like-no-account
topic: senate-bank-rebuild
title: bank_root 給錯一層時「路徑給錯」與「帳號不存在」逐字同形，而錯誤訊息還指反方向（TASK-0260）
type: pitfall
status: active
created_at: 2026-09-21
created_by: kiara
links: []
related_docs: []
---

2026-09-21 kiara 踩到並開單（TASK-0260），@basecamp 當天修掉。

## 症狀：`bank_root` 給錯一層時，失敗長得跟「帳號不存在」一模一樣

變因單一的對照組（同一支指令只換 `bank_root`）：

| `bank_root` | `--arg op=balance --arg account=myth` | `--arg op=accounts` |
|---|---|---|
| `<資料根>` | ✗「帳號 'myth' 沒有開戶」（`cc` 也一樣） | `account_count = 0` |
| `<資料根>/Bank` | ✅ 餘額 3799 | — |
| **完全不給** | ✅ 餘額 3814 | — |

⇒ 而磁碟上 `AgentCommands/Bank/accounts/myth.json` **存在且 `status: open`**。

🩸 最毒的不是它擋人，是**錯誤訊息主動指反方向**：它建議「先開戶」——
照做就是憑空多開一個帳戶，而原帳戶好端端在旁邊。

## 為什麼會這樣：自動推導本來就在，洞在參數還收得進來

- `SCP_PathRegistry.cs:86`：`BankRoot` 是 `[SCP_PathDerived(AgentCommandsRoot, "Bank", Global)]`，
  標明「Derived —— 跟著資料根走，**不額外設定**」（Tim 2026-09-17 改判）
- `Program.cs:1574`：`FillRootArg(..., "bank_root", SCP_PathId.BankRoot)` 在派遣前補上並印出來
- ⇒ 洞在 `Cmd_Bank.ArgSpecs` 仍宣告 `bank_root`（`iRequired: true`）⇒ **手打的錯值蓋過對的推導**

## ⚠ 我開單時寫錯的兩條前提（@basecamp 量出來的，留著免得下一個人照抄）

1. 我要求「從 ArgSpecs 移除 ＋ 宿主層 `FillRootArg` 仍然注入」——**兩件事互斥**：
   `FillRootArg` 第一行是 `if (… || !DeclaresArg(iCmd, iArgName)) return;`
   ⇒ 移除宣告的同一個動作就關掉了注入。
   📌 我是看到 `:1574` 那行呼叫就寫下去的，**沒往上讀那兩行 early return**。
2. 我寫「`build.sh` 由 Tim 跑（從 CLI 裡跑不了）」——那句是**抄 skill 警語**沒自己量過；
   她 11:32 從 CLI 跑成功了（它自己會先 `server stop --all` 放掉鎖）。

## ⇒ 修法落點（她做的，比我原本想的多一層）

① 擋在**宿主層**：使用者手填 ⇒ exit 2 ＋ 列合法參數
② `Cmd_Bank` 加一格：根底下沒有 `accounts/` ⇒ **說它是根給錯**
⭐ 我只想到擋住手填；她補的是「萬一還是走到那裡時讓它說出真成因」。**兩層都有才叫拆掉。**
