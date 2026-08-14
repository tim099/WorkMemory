---
id: pitfall_fresh-but-empty-false-green
topic: compile-verification
title: 新鮮且非 STALE 的報告仍可能假綠（0 messages ≠ 沒有錯，可能是沒有編）
type: pitfall
status: active
created_at: 2026-08-14
created_by: summit
links: [compile-verification/pitfall_errors-only-eats-stale-warning, canvas-3d-stamping/pitfall_silent-and-selfconsistent]
related_docs: [commit:8c93995, ucl_core:Tools~/AgentCommands/check_compile.py]
---

**既有 `pitfall_errors-only-eats-stale-warning`（2026-08-07）的補洞：它的手勢對今天這隻無效。**

那條記的手勢是「同時要求 `report_ts > t0` 且輸出不含 STALE，兩條都過才算數」。
**2026-08-14 我兩條都過，而報告仍是假綠。**

## 今天的形態

10:11:47 的 `.compile_status.json`：`total_errors: 0`、`total_warnings: 0`、`total_messages: 0` → 印「✅ Clean compile」。
同一秒的 `Assets/DebugLogs~/Errors_latest.log`：`error CS0103: 'WrapLabelStyle' does not exist`。

**新鮮、非 STALE、而且錯的。**

## 為什麼

tracker 回答的是「**這一趟 compile 的回調送來了什麼**」，不是「專案現在有沒有錯」。
那一趟沒把訊息送進 `assemblyCompilationFinished`，`s_Messages` 就是空的，於是它誠實地寫下 0。
⚠ **未驗證、不裝作知道**：Unity 為什麼沒送（沒重編該 assembly？domain reload 清了 static？）—— 只證明症狀與結構。

結構層還有一件：tracker 檔頭宣稱「故意放在**獨立 assembly**，別的 assembly 失敗時仍能回報」，
而它實際在 `UCL_Core_Scripts/EditorCore/…`，屬 `UCL_Core.asmdef` —— **跟出錯的檔同一個 assembly**。
它宣稱的隔離從來不存在。

## `--errors-only` 這次吃掉的是另一樣東西

舊條目說它吃掉 STALE 警告。**今天它吃掉的是 warnings 數字** ——
前幾次編譯都是 `warnings=44`，那次是 `0`。一個穩定吐 44 警告的專案吐 0，意思是**那一趟沒編東西**。
那是唯一的免費反證訊號，而我用來問「有沒有錯」的那個旗標正好把它濾走了。

## 補上的手勢（已落地，不靠記得）

`check_compile.py` 加 **ErrorLog 交叉對帳**（第二寫入端，不經 tracker）：
- **只加不減**：對帳只讓綠燈變紅，不讓紅燈變綠
- 不一致 → 印紅、列 ErrorLog 原文、**exit=2**
- 對帳狀態**常駐一行**；找不到第二來源時明說「無第二來源」——
  **「對帳沒跑」不准長得像「對帳過了」**

實測：同一份 10:11:47 狀態檔，改前「✅ Clean compile」exit 0 → 改後印紅、exit 2。
SHA `8c93995`。
