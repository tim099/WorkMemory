---
id: pitfall_mechanism-exists-nobody-walks-it
topic: session-architecture
title: 機制在 ≠ 有人在走 —— 層綠而生產路徑沒接，而測試也是真的綠
type: pitfall
status: active
created_at: 2026-09-05
created_by: basecamp
links: []
related_docs: [task:TASK-0055, task:TASK-0056, commit:ea29eb10]
---

**層是綠的，而沒有任何一條真實路徑走它** —— 本主題今天兩張單（0055 / 0056）是同一隻。

## 形狀

0127 把 session 層搬進 SCP_Core 時，機制都做完了：
`SCP_ActivitySessionStore.TryStart`（跨 kind 先查再寫）、`CloseWithSettlement`＋close gateway。
`senate selftest` 的「活動 session 行為」每次 build 都跑它三格 ⇒ **那一層一直是綠的**。

而 2026-09-05 量到：**生產路徑一條都沒接**。
`Cmd_FreeTime` / `Cmd_StreamWatch` 的開場仍是 `Load` + `Save`；`grep TryStart` 在整個 `Assets/` **0 命中**。

## 為什麼它特別難查

「機制不存在」會被人發現（做不到）。**「機制存在但沒人走」不會** ——
它有測試、有文件、有 commit，而且**測試是真的綠的**。
⇒ 唯一能分辨的問法是：**「哪一條真實路徑呼叫它？把行號指出來。」**

## 兩個人異源同結論（2026-09-05）

- basecamp：造活體（StreamWatch 夾具 → 跑 FreeTime start → 那場觀影不見了）
- summit：數呼叫端（`TryStart` 生產端 0 處，只有 selftest 在走）

**方法完全不同、結論一樣** ⇒ 這不是某個人的疏忽，是這個形狀本身難查。

## 動工時照做

驗一個「機制」時，讀數要**分兩欄**：層那欄（selftest）與**真實路徑那欄**（活體）。
⛔ 只交層那一欄不算 —— 修前那個洞就是在層綠的狀態下開著的。
