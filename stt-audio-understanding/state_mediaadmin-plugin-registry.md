---
id: state_mediaadmin-plugin-registry
topic: stt-audio-understanding
title: 影音管理頁改插件註冊表＋下拉選單（含解除安裝）— 已 commit 88e4358
type: state
status: active
created_at: 2026-08-11
created_by: summit
links: []
related_docs: [ucl_core:Docs~/zh-Hant/UCL_EditorPage/UCL_MediaAdminPage.md]
---

## 現況（2026-08-11 收工時）

**已落地並 commit（`88e4358`，UCL_Core Dev，單層未 bump 父層）**：
影音管理頁第 2 區從「四顆固定安裝按鈕」改成**插件註冊表 + 下拉選單**。

- **唯一定義處**：`media_admin.py` 的 `PLUGINS` dict
  （`stt` / `torch` / `ocr`，各帶 `name/desc/probe/actions[{id,label,hint,danger}]`）
- **新 op**：`list-plugins`（JSON：清單＋即時 import 探測＋動作）、`plugin --id X --action Y`
- **舊 `install --stt/--torch-cuda/--ocr/--ocr-cuda` 子命令已移除**（同一件事兩個實作＝漂移）
- C# 端 `UCL_MediaAdminPage` 跑 `list-plugins` 建 UI → **新增插件只改 python 那張表**
- danger 動作跳確認框，框內原樣列出該動作的 `hint`

## 拍板（Tim 2026-08-11）

> 插件會越來越多，不要繼續一顆一顆加按鈕 → 改成「下拉選插件 → 顯示該插件的動作」。

我原本要加的是四顆解除安裝按鈕（加邏輯），Tim 換掉框架。**這是當天第七次同形狀。**

## 兩條刻意擋起來的坑（改 code 前必讀）

1. **共用套件不進任何插件的卸載清單**：`numpy` / `torch` 被 daemon / montage / audio-viz 共用，
   夾帶卸掉會**靜默弄壞整條陪看鏈**。`stt/uninstall` 只卸 whisper + soundcard；torch 另立插件。
2. **卸載必須迴圈到乾淨**：pip 一次只卸 sys.path 順位最前那份，user-site 與 system site 可能各有一份
   （torch 孤兒前科）。`_pip_uninstall()` 迴圈到 pip 不再回報 `Successfully uninstalled`。
   **只跑一次會留下被遮蔽的第二份，而 status 仍顯示 ✅ —— 那個「解除安裝成功」是假的。**

另：`ocr/cpu` 是**降級不是移除**（卸 gpu dist → 裝回 CPU 版，OCR 仍可用），命名要對得起事實。

## pending（給接手的人）

- ⚠ **uninstall 實跑未驗** —— 會拆掉現在能用的環境，我沒跑。要驗請在頁面上按（有確認框），
  或拿一個無關緊要的套件試迴圈邏輯。
- **父層 submodule 指標仍指著舊 hash**（單層 commit）—— 若已 `commit all` 則此條作廢。
- 下一個可能的插件來源：basecamp 的音訊理解鏈 v2（分離器 demucs / diarization pyannote）
  —— 那條鏈要新依賴時，**加進 `PLUGINS` 就有 UI，不必動 C#**（這是本次改動的主要收益）。
