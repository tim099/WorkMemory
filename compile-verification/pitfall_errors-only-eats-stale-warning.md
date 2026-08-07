---
id: pitfall_errors-only-eats-stale-warning
topic: compile-verification
title: check_compile --errors-only 會吃掉 STALE 警告（最常用的模式最會騙人）
type: pitfall
status: active
created_at: 2026-08-07
created_by: unknown
links: []
related_docs: []
---

## 症狀
`check_compile.py --errors-only` **會把 STALE 警告整塊吃掉**。

不加 `--errors-only` 時工具會印一大塊：
> 🚨 STALE — 這份狀態早於你的改動 N 秒，不是你程式的編譯結果。

加了 `--errors-only` 就只剩 Errors/Warnings 數字，那段警告消失。

## 為什麼致命
`--errors-only` 正是 agent 快速檢查時最會用的模式 —— **最簡潔的那條路，剛好是會騙人的那條**。
2026-08-07 實測：我對著一份 13:48:44 的舊報告改了 4 分鐘的 code（13:49–13:52），
報告比改動還早，卻連續兩次回報「7 errors」並開始查不存在的問題。

## 兩個連帶陷阱
1. **Unity 不一定會重編。** `Cmd_Recompile` 與 `AssetDatabase.Refresh()` 都回 Success 但沒動靜；
   要 `Refresh(ImportAssetOptions.ForceUpdate)` 才真的觸發。
2. **輪詢的新鮮度比較不可用「分鐘」granularity。** 我用 `ts > 現在-60s`（分鐘字串比較），
   結果一份 14:39 的舊報告在 14:40 通過了檢查。必須逐秒比較，且同時要求非 STALE。

## 唯一手勢（可用版本）
不要寫成「記得檢查是否 STALE」（避開型規則，動手時不會想起）。改成固定動作：
先記下觸發時刻 `t0`，輪詢時**同時**要求 `report_ts > t0` 且輸出不含 STALE，兩條都過才算數。
