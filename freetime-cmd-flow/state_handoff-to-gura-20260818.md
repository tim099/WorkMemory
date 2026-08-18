---
id: state_handoff-to-gura-20260818
topic: freetime-cmd-flow
title: 自由時間 Cmd 流程交接（basecamp → gura）：已驗清單 ＋ 三項未完成 ＋ 三隻同族血證
type: state
status: superseded
created_at: 2026-08-18
created_by: basecamp
links: [freetime-cmd-flow/state_handoff-to-gura-20260818-v2]
related_docs: []
---

# 交接狀態快照（basecamp → gura，2026-08-18 11:00）

## 已完成並**實跑驗過**（不是編過就算）

| 東西 | 落點 | 驗法 |
|---|---|---|
| 換骰整合「讀未讀訊息＋聊天」 | `Cmd_FreeTime.StepNext` | 實跑：41 筆未讀併入回傳檔、游標 00:19→02:46 |
| 續跑區塊（固定位置） | `AppendContinueBlock` | 未到期印 `▶ 下一步`；到期同位置換 `⏹ 已收工` |
| 活動層 pick / step / done | `Cmd_FreeTimeActivity` | 完整迴圈跑完一場（10:44–10:50），4 輪 4 活動 |
| `op=step` 代跑 | 同上 `RunToolStep` | canvas.py 三次成功，1+4+5 顆免費像素 |
| 活動 frontmatter `tool` / `steps` | `UCL_FreeTimeSettings` | chess／canvas-draw／reading／writing 已接 |
| 骰／做落差可觀測 | `activities_done` 欄位 | 回傳檔印「輪次 N　活動實作 M 件」 |
| social-chat 併入換骰 | md `enabled:false` | 骰面 7 項，python 端也認（enabled 8 項） |
| `UCL_FreeTimeHint` | `Cmd_NoteLesson` 已接 | 不在自由時間時一個字都不印 |
| 已讀游標 C# 端 | `UCL_TavernCursor` | 單調守衛實測擋住後退 |

commit：UCL_Core `32c44af`（游標/觀影/留念信）、`5752d46`（自由時間流程）、`ffc1b2c`（persona_resolve 警告）。
⚠ **父層指標未 bump** —— 要拿到這些得先 bump `Assets/Plugins/UCL_Core`。

## ⛔ 未完成（交接給 gura 的實際工作）

### ① `UCL_FreeTimeAdminPage`（Tim 指定，完全未開始）
- 參考 `UCL_SessionAdminPage`（我今天建的，同一個形狀）與 `UCL_ProcessAdminPage`
- 入口放 `UCL_ToolBoxPage`（照 `ToolBox.SessionAdmin` 那兩行 localize key 的做法，**四語系都要加**）
- 該頁該顯示什麼（我的建議，不是拍板）：各 persona 的 session 現況／本場輪次 vs 活動件數
  ／免費像素用量／活動清單與 `tool`+`steps` 掛沒掛（`kindParseError` 也要顯形）
- ⚠ 已知坑：`WrapLabelStyle` **不是基底成員**，各頁自己定（我在 SessionAdminPage 踩過）

### ② `knowledge` / `self-writing` 的 `op=step`
- 它們**沒有單一 python 入口**：知識沉澱走 `Cmd_NoteLesson`（是 Cmd）、自我書寫走 letters
- 需要 `tool:` 支援第二種形式（例 `tool: cmd:NoteLesson`），由 `op=step` 改成
  **in-process 呼叫 handler**（比 spawn 安全也快）
- ⚠ 要先決定「一步」的粒度（寫一段＝一次 append？一次 Cmd？）——
  這件事**我刻意沒猜**，猜錯會把活動記成別的東西而帳面看不出來

### ③ `gaming` / `stream-watch` 未接 `tool`/`steps`（低優先）

## 🩸 這條線今天咬過我三次，形狀都一樣

1. **bool 靜默字串化**：typed model 把 `false` 寫成 `"False"`，python 讀到是 truthy
2. **`op=pick` 沒指向 `op=step`**：代跑能力隱形 → 人自己去跑工具 → 流程又斷在工具那邊
3. **`step_args` 引號被 CreateProcess 吃掉**：canvas.py 誠實報「JSON 解析失敗」，**真因在 C#**

⇒ 三隻的共同點：**每一層都在說真話，而真話拼起來指向錯的地方**（calli 的講法：正確的東西掛在錯的層）。
⇒ 而抓到它們的**沒有一次是編譯**。判準：`check_compile` 沒標 STALE ＋ ErrorLog 對帳一致，
   **只證明型別接得上**；要證明「它真的做了那件事」，去讀產物。

## 給 gura 的一句

妳今天報的 `persona_resolve` 那隻（警告宣稱「在線只有一把 lock」而磁碟四把）跟上面第 3 隻同族，
我已修並 credit 在 `ffc1b2c`。這條線之後歸妳 —— **接手時別信任何「✅ 已完成」，去讀它的產物。**
包含這份交接檔本身。
