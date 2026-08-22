---
id: state_state-day1
topic: senate-backend
title: Day 1 現況：能跑能看能操作，三格未驗
type: state
status: active
created_at: 2026-08-23
created_by: basecamp
links: []
related_docs: [D:/Unity/Senate/Docs/DOC_INDEX.md, D:/Unity/Senate/README.md]
---

2026-08-22 起手，一天內從零到「可以跑、可以看、可以操作」。

## 兩個 repo（都在 Unity 專案外部）

- **Senate** `D:/Unity/Senate`（github.com/tim099/Senate）—— 後台本體，.NET 10
  - `src/Senate.Core`（設定／git CLI／專案探測）
  - `src/Senate.Desktop`（ImGui renderer／視窗／字型／截圖；Silk.NET）
  - `src/Senate.Cli`（headless 入口＋後台頁面）
- **SCP_Core** `Senate/SCP_Core`（github.com/tim099/SCP_Core）—— Unity 與 .NET **共用** submodule
  - `Runtime/Json`（值樹＋parser＋writer）
  - `Runtime/Gui`（UI 中間層：節點樹／撰寫 API／文字 renderer／非 UI 操控介面）
  - **已裝進 Bar 的 `Assets/Plugins/SCP_Core`**（Tim 自己裝的，並 commit 了 8 個 .meta）

## 現在能做什麼（都有實跑讀數）

| 指令 | 狀態 |
|---|---|
| `./setup.ps1` / `setup.sh` | 一鍵配置：檢查前置→build→建本機設定→doctor |
| `./build.ps1` / `build.sh` | 一鍵 build：publish→根層 `senate.exe`→**真的跑一次 doctor＋真的開一次窗** |
| `senate doctor` | 環境＋各專案讀數（含 git 分支／dirty／staged／Editor 心跳） |
| `senate selftest` | SCP_Core JSON 層對拍（拿 Unity 寫的 commands_schema.json 跑 round-trip） |
| `senate ui --window` | ImGui 視窗；**雙擊 senate.exe 也會直接開**（判準 GetConsoleProcessList） |
| `senate ui --list/--click/--set/--toggle/--json` | 非 UI 操控介面（agent／腳本可驅動，session 存 build/ui_session.json） |
| `senate ui --screenshot <png>` | 開窗、畫幾幀、存 PNG 後自關（給沒有眼睛的人驗收） |

## 尚未驗的三格（明天的第一批）

1. **UI 中間層在 Unity 編不編得過** —— Bar 的 SCP_Core clone 停在只有 JSON 層的那顆；
   JSON 層已確認編過（`Library/ScriptAssemblies/SCP_Core.dll` 真的生出來）。
   重點：`init` 存取子＋自補的 `IsExternalInit` polyfill 在 Unity 的 C# 9 吃不吃。
2. **中文 IME 輸入** —— 畫面上還沒有輸入框可以打字。ImGui 這條路的存亡押在這格。
3. **文字 renderer 的表格不吃 `--width`**（欄寬取自然寬度，窄視窗會超出）。

## 下一個功能：`senate autocommit scan`

規則資料化（寫死的 C# → 入版控的 profile JSON，Unity 端改成讀它）。
第一個真關卡是**離線對拍**：把 `git status` 原文存成 fixture，兩邊吃同一份跑分類，`diff` 要空。
⚠ 動工前先補格式：既有 `.ucl_autocommit.json` **只吃前綴**，表達不出 `letters_mech` 那條
exact match（`p == "_latest.md"`）⇒ 要加 `exactPaths`，不是照抄。
