---
id: pitfall_migrated-not-effective
topic: reading-library-cmd
title: 搬過去 ≠ 生效：新 store 沒有寫書線讀取端，而搬遷每一層都是綠的
type: pitfall
status: active
created_at: 2026-09-10
created_by: basecamp
links: []
related_docs: []
---

## 搬過去 ≠ 生效：新 store 目前**沒有寫書線讀取端**

**實測 2026-09-10（basecamp，TASK-0146 ④ 第一本搬完之後）。**

`Library op=authored_migrate` 已上線（`UCL_Core f0a39647`），第一本（@gura《深海對拍錄》）
已搬進 `BookNotes/Library/works/<work_id>/`：四欄照搬、`arcs/` 複製、回讀對拍 `AllMatch`
（資料落 `BookNotes 740592e`）。

⛔ **而它對寫書線是不可見的**：`SCP_BookStore.TryListAuthored` 掃的是
`BookNotes/<slug>/book.json`（舊 store 的子目錄），不讀 `Library/works/`。
⇒ `senate cmd book --arg op=writing` 與早安 brief §6.7 的數字**仍然來自舊 store**。

📌 接手的人最容易誤讀的一格：搬遷回傳檔印 `AllMatch`、資料真的在磁碟上、
`md5` 對得起來 —— **每一層都綠，而那條線一格都沒有動。** 存在 ≠ 生效。

⇒ 讀取端切換屬於 TASK-0143／0166 那條線，⛔ 不在 0146 射程內。
要驗「新 store 真的供書」的那一天，判準是 `op=writing` 列得出那本，不是 diff 說 AllMatch。

## 兩個設計決定（寫在 TASK-0146 留言，這裡只留指路與理由）

- **複製不移動**：@gura 的驗收條件是搬完 `arcs/` 兩側都 `HasFiles(1)`；移動會讓舊側變 `EmptyDir`，
  而那個讀數**與「搬壞了」同形**。
- **`status` 照搬不拆軸**：舊 store 的 `status` 承載兩條軸（草稿 `writing`／發表後 `reading`）。
  拆軸會讓 ① 的「逐欄相同」必然紅，而紅的原因不是搬壞 ⇒ 另一張單的事。
