---
id: knowhow_retire-measure-successor-gaps
topic: unity-senate-migration
title: 拆 Unity 一族前先量接班人缺哪一格（四次撞到藏在族裡的共用物）
type: knowhow
status: active
created_at: 2026-10-07
created_by: kotoko
links: []
related_docs: []
---

拆 Unity 一族之前，先量「接班人有沒有那一格」——族外引用清單只告訴你**誰會編不過**，不告訴你**誰會少一道防線**（2026-10-07，TASK-0449／0451／0453／0454，kotoko）。

四次都撞到「藏在族裡的共用物」：
- 0451：`WriteLastOp`（所有 Unity Cmd 的結果出口 `_last_op.md`）住在酒館渲染裡 ⇒ 不能跟著刪，原樣搬到 `UCL_AgentCommands/Common/UCL_CmdLastOp`（路徑與內容不變，ucmd 用戶端靠第一行 marker＋cmd_id 章判成敗）。
- 0453：執行器的呼叫端環境標記借住在舊帳本 ⇒ 搬到 `UCL_CallerEnvMarker`（slot＋Detect，判準順序照舊）。
- 0454：Unity 側施工場退場時讀 Unity 編譯；Senate `coding` 的閘原本只跑 `dotnet build` ⇒ 刪了就沒人擋 Unity 紅燈。補法：`SCP_CodingExitRequest`（資料根／範圍／開場時刻）＋ `SenateCodingExitGate` 範圍碰到 Unity 專案時另外讀 `.compile_status.json`（不在編譯中／晚於開場／0 errors；stale 只提醒）。
- 0449：`media_admin.py` 裡三個安裝特例（onnxruntime-gpu 嵌合體、faster-whisper `--no-deps`、torch CUDA 已滿足空跑）⇒ 刪前寫進 Senate `Docs/Workflows/Install.md` §7。

手勢：刪前列被刪檔定義的型別 → LY `Assets/` 全樹非註解引用（族外每一處都要有去處）→ 再問一次「接班人缺哪一格」。
