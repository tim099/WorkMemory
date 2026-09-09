---
id: pitfall_build-window-in-agent-process-tree
topic: senate-backend
title: build 收尾那顆常駐視窗在 agent 的行程樹裡＝永不結束的 GUI 子行程（ClaudeCode 關不掉／重啟被占用）
type: pitfall
status: active
created_at: 2026-09-09
created_by: basecamp
links: []
related_docs: [Docs/Workflows/Setup_And_Build.md, commit:9427ca2, tavern:2026-09-09#17015]
---

## 現象（Tim 2026-09-09 報的）

**ClaudeCode 會被關閉，而且無法重啟（提示「被占用」），必須先關掉 Senate 才起得來。**

## ⛔ 先否證掉的那條

**Senate 不會主動殺它。** 全部 `Get-Process` / `taskkill` / `Kill()` 呼叫點掃過：
`build.sh` / `build.ps1` 那段收視窗的 PowerShell 是 `Where-Object { $_.Path -eq $t }`（**比路徑**，
只收自己那顆 exe 開的視窗）；`SCP_ProcessRegistry.KillRegistered` 只殺登記過、且過
PID＋name＋start time 三重驗證的。⇒ 不要再往「它殺我」那個方向查。

## 假說（⚠ 是假說，handle 繼承本身**沒有量到**，也沒有重現一次關閉）

`build.sh` 收尾那行 `nohup "$exe" ui --window > … &` 開出來的是**呼叫端 shell 的子行程**。
呼叫端是 agent 時，它就是 **ClaudeCode 行程樹底下一顆永遠不會結束的 GUI 行程**，
繼承了那條 shell 的 handle ⇒ 前者關不乾淨、重啟時鎖還被握著。
📌 從**檔案總管雙擊**開的那顆不在那棵樹裡 —— 這解釋了為什麼同一個動作只有「有時候」會壞。

## 修法（`Senate 9427ca2`）：判準不是「開或不開」，是「我是不是站在一個人的終端機前面」

| `build.sh` | 行為 |
|---|---|
| `stdout` 是終端機 | 照 Tim 2026-09-04 拍板**開** |
| `stdout` 不是終端機（agent／導向） | **一顆都不開** ＋ 印理由與 `--window` 出口 |
| `--window` / `--no-window` | 強制開／強制不開 |

- ⛔ **不做「開了再自己收掉」**：那是在 Claude 樹裡先種一顆再拔，拔的那步失敗一次就回到原病。**不種就不必拔。**
- ⛔ **不靜默**：三條路各印一行，不然「這次沒開」與「開了但當掉」在畫面上同形。

## ⚠ 交接必須知道的兩格未量

1. **`auto`＋有 TTY 那一支沒有讀數** —— agent 定義上沒有 TTY（`winpty` 也造不出來，這個 sandbox 沒有真 console）。
   ⇒ **要人在自己的終端機跑一次 `./build.sh`** 才有那一格。
   📌 風險方向是對的：判錯的失效樣子是「視窗沒開」，一個 `--window` 就解決 —— 不會是 Claude 被關掉。
2. **`build.ps1` 刻意沒跟上**（兩邊行為已不同，理由寫在它自己的註解裡）：本環境 agent 跑不了 PowerShell
   ⇒ ① 那條路目前沒這個病 ② 改完的樣子驗不了。而「.ps1 與 .sh 等價」是 2026-08-22 咬過的那格。

## 🩸 同族還有一條沒動

`senate server start` 若被 agent 用背景方式起（2026-09-09 早上我就這樣起過一顆），
形狀一模一樣：**永不結束、在呼叫端行程樹裡**。⇒ 要改的是 server 的語意，等 Tim 判。

## 而副作用要知道：`build.sh` 開頭那段會關掉**別人正在用的**視窗

它比路徑、只收自己那顆 exe 的視窗 —— 規矩上對。但 2026-09-09 我從 Claude 裡跑了五次 build，
**也就是關了 Tim 五次視窗**，而畫面上每次只有一行 `· 收掉 1 顆還開著的 senate`。
⇒ 那行字沒有錯，只是它從來不說「那顆是誰的」。
