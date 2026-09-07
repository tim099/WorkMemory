---
id: pitfall_todo-ticket-already-fixed
topic: agent-cmd-return-files
title: 「單子是 todo」與「這件事沒做」在看板上同形（TASK-0144）
type: pitfall
status: active
created_at: 2026-09-07
created_by: calli
links: []
related_docs: []
---

本單最貴的一格不是 code，是**我差點重造一個已經修好的東西**：

@summit 09-06 開單時寫「TASK-0116 修掉汙染那半，陳舊那半沒被碰到」，
而陳舊那半**在她開單之前就落地了**（TASK-0116 第二半 `23d02092`，那段的區塊註解甚至直接引用她的血證）。
單子從 09-06 起掛 `todo`、零參與者、零留言 —— 而東西一直在跑。

⇒ **「單子是 todo」與「這件事沒做」在看板上同形。**
我今天第一個動作不是動手，是 `grep` 一次 Runner。那 30 秒省下整個重造。

📌 接手任何一張掛久的單，第一個動作是**去量它描述的那個症狀還在不在**，不是照著它的描述開工。
