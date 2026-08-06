---
id: decision_spec-2026-08-06-six-questions
topic: reading-library-cmd
title: 六題定案：讀寫同框架在 C#、讀回自動、遷移要人、time_range 是事實
type: decision
status: active
created_at: 2026-08-06
created_by: summit
links: []
related_docs: []
---

## 六題定案（2026-08-06 酒館三讀，seq 14609 / 14611 / 14613 / 14614 / 14615）

寫入端做成 C# Cmd（`Cmd_Library`），Python 只當薄 client 走 `run_cmd.py`。
理由：發文要走 `Cmd_Tavern` 同一條路徑才不漏 mirror / inbox 路由 / mention 解析 / 計酬判定；
in-process 拿 seq，不必 spawn run_cmd 再刮 stdout。

| 題 | 定案 | 來源 |
|---|---|---|
| Q1 讀回 | **找到自己的紀錄→直接讀回並放行**；沒有→停下來問 Tim，或由 Tim 指定 `relay_from=<persona>` 接力別人的心得 | Tim 拍板，**推翻** basecamp/gura/我三票的「硬擋跳章」 |
| Q1' 跳章 | 不擋。相等＝重讀(開 r2)／+1＝續讀／其他＝gap，**在 chapter.json 記 `gap: true` 留痕不靜默**。0000 序章不參與連續性判定 | basecamp 判準降級為分類 |
| Q2 讀回實作位置 | ⚠ **反轉**：讀回也要在 C#，讀寫同框架避免漂移。Python `library.py reading-recall` 必須退成薄 wrapper 或退役，**不可並存** | Tim（我原案 Python 讀 / C# 寫本身就是兩個實作） |
| Q3 Archive 未遷移 | 沒有新心得但查到舊心得 → **停下來**，跑 migration 手動搬。偵測自動、遷移不自動 | Tim + basecamp（猜錯原讀者＝替別人代筆閱讀史） |
| Q4 Archive 命中判準 | `op=scan` 先印候選人工核對。前綴法誤報 60%、title 法漏一半（08-05 實測） | 三票一致 |
| Q5 遷移 receipt | `_migration/registry.json` 補 fingerprint 演算法 / 輸入清單數量 / 輸出 target paths（Sirius 第三判決）。「現在補是加三個欄位，之後補是回溯考古」 | basecamp |
| Q6 電影章節 | `time_range`（`00:00-30:00`）是**事實**建議必填（Tim 手動切段留下的）；`display_number` 填人話可選、缺則由 chapter_id 派生 | basecamp 量到既有樣本 display_number 已退化成 chapter_id 複寫 |
| media_kind | enum = media-id 前綴同字：`comic/anim/film/series/stream/book`。另造 `movie` 等於同一件事兩個名字 | Tim + basecamp |
| 章節編號 | 四位數，**0001 起算，0000 保留序章（非必有）** | Tim。dungeon 的 0000 正是連載前短篇 —— 我一開始誤判它違規，已收回 |
| 落檔 vs 發文 | **檔優先**。心得是事實源、酒館貼文是投影；發文失敗回滾心得＝銷毀掙來的真去保投影一致性 | basecamp 給的理由，比我原本的好 |
| 頁面 | `UCL_ReadingNotesManagePage` 要有讀取功能，與 Cmd 的讀取**走同一段**（服務層），Tim 協助 QA | Tim |

## 架構（單一 schema 實作者）

```
UCL_ReadingLibraryIO      ← 路徑 + JSON 讀寫 + media_init/note_chapter/bookmark + RenderRecall
   ├── Cmd_Library                     （agent 入口 / RPC 包裝）
   └── UCL_ReadingNotesManagePage      （人的入口，Tim QA）
```
JSON 一律走 `UCL.Core.JsonLib.JsonData`（保序 DOM），**不用 JsonUtility** —— 後者會把型別沒宣告的欄位靜默吐掉，而 reader.json/chapter.json 是多方共寫的檔。
