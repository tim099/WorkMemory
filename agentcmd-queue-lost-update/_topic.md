---
id: agentcmd-queue-lost-update
title: AgentCommand queue.json 的讀改寫與讀取三態
status: active
created_at: 2026-09-23
related_topics: []
key_docs: []
task_indices: [264]
---

TASK-0263/0264：queues/<persona>/queue.json 多寫入端無互斥，以及讀取端把「讀不到」壓成「空」的那一族。本主題收的是判準與踩坑，進度在 Task 時間線。
