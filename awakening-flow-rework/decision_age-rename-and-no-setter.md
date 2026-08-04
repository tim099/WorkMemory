---
id: decision_age-rename-and-no-setter
topic: awakening-flow-rework
title: wake_count → age（Tim 拍板）；且 age 不可寫入、只可推導
type: decision
status: active
created_at: 2026-08-05
created_by: basecamp
links: []
related_docs: []
---

**拍板（Tim 2026-08-04）**：`wake_count` 改名 **`age`**，不用 `completed_wakes`。

**語意**：`age` = **已完成**的 wake 數（= `wakes/` 收尾信數）。「這次是第幾次醒來」= `age + 1`，**永不落地**。

**為什麼 `age` 比 `completed_wakes` 好**（兩條，第二條是 Tim 沒明說但成立的）：

1. **年齡天生帶那個 off-by-one 的慣例** —— 52 歲的人正在過他的第 53 年。不必寫文件解釋「數的是完成的還是含當下這次」，這是人類文明已經安裝好的直覺。
2. **`age` 只在「好好收工」時增加 → 停止被寫入的世界線停止老化。** 那條停在 07-28 的 `worldlines/20260617-a`，它的 age 從那天起凍住。這件事用 `wake_count` 講不出來（「醒來次數」聽不出「停止」的意思）。

**追加拍板（Tim 2026-08-04 QA）**：
- **`age` 不可寫入，`set_persona_age()` 必須移除** —— 一個「能推導所以不該存」的值留 setter 就是自相矛盾，只要有 setter 就還有第二個真相源。改為 `derive_age(persona)` 唯一真相源 + `export_age_cache()` 單向匯出給非 python reader（C# 管理頁）。
- **相容性硬要求**：舊格式 persona（收尾信還在頂層、`wakes/` 為空）**不能被當成 age=0**。`derive_age` 必須兩種擺法都認。

**實測背書**：10 個已遷移 persona 的純推導值與當時存的值**一個不差**（那欄對他們資訊量為零）；9 個未遷移的存值與證據**兩個方向都對不上**（crest-001 存 22／信 28、ridge-001 存 12／信 8）。

**施工狀態**：整份 P1 在 `Assets/Plugins/UCL_Core` 的 `stash@{0}`，**等所有 persona 遷移到 `wakes/` 新格式後再測**（Tim 2026-08-04 決定暫緩）。理由見同主題 pitfall。
