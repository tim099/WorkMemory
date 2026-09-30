---
id: knowhow_task-writer-moved
topic: unity-senate-migration
title: 任務單寫入搬到 Senate：形狀與搬『寫』時會撞的三格（TASK-0349）
type: knowhow
status: active
created_at: 2026-09-30
created_by: calli
links: []
related_docs: []
---

**任務單寫入搬到 Senate（TASK-0349，2026-09-30 已結）—— 形狀與下一個搬「寫」的人會撞到的三格。**

- 形狀照酒館：入口 `senate cmd task`（驗參數／回傳檔／酒館通知／work_memory.py 代跑）→ Server `task-write`（唯一寫入端，固定 lane `task`，參數整包 `args_json`）→ SCP_Core `SCP_TaskOps`（op 本體）＋ `SCP_TaskStore`（Mutate／Create／Link，兩把鎖：Monitor ＋ `SCP_FileLock`）。
  ⚠ **要等別的 process 的事（發文、python、開 Coding 場）不在寫入端做**：持鎖等人會卡住所有寫入 ⇒ 寫入端只組好、交回入口，寫完才做。
- Editor 端是**刪掉寫入面**而不是留著轉交：`UCL_TaskIO` 的 Save／Mutate／Create／Link 整段刪 ⇒ 留著的 public 寫入 API 會被下一個人順手呼叫。Editor 的寫入入口（Cmd 寫入 op、後台頁、晚安 skip）改 spawn `senate cmd task`（`UCL_TaskSenateBridge`，照 `DelegateAppendToServer`）。
- 🩸 **搬寫入端時，拿新寫入端把所有舊資料重寫一次逐位元組對拍** —— 那把尺抓到的不是新的錯，是**舊寫入端**的：區塊標題用前綴比對，0126／0204 的結單說明一直在被截斷，只是沒人重寫過它們。selftest `RealTaskRenderRoundTrip` 是範本（容許的差異要逐類列名：缺鍵補空行／缺區塊／多餘空行，其餘算真不符）。
- 配號換形狀的判準：**建檔成功才寫計數檔** ⇒ 建構丟例外時結構上不吃號（0351／0352 那族）；計數檔降級成快取，事實是檔名。
- Tim 2026-09-30 的判準更正：「不需要 Editor」的驗收＝**流程中不用到 ucmd**，不必真的關 Editor。
