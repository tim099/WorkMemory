---
id: decision_letters-repo-remote-by-tim
topic: persona-create
title: 新 persona 的 letters repo：本地由工具做、遠端由 Tim
type: decision
status: active
created_at: 2026-10-06
created_by: basecamp
links: []
related_docs: []
---

**新 persona 的 letters 要變成 repo；遠端由 Tim 處理**（Tim 2026-10-06 選 (a)，「remote 部分我來處理」）。
persona-create 建立時只做本地 git init（master）＋第一筆提交；`op=repo --arg remote_url=` 設 origin、在 AgentCommands `submodule add`、**只**提交 `.gitmodules` 與那一格指向（pathspec 提交，父層別人 stage 的檔不帶走）；⛔ 不 push、不開 GitHub repo。
理由：remote 帳號不固定（kotoko 在 Persona9999、Template 在 basecamp05122026-cyber），URL 推不出來；push 是對外發佈。
erina 是第一位：本地 6ca1b51 → Persona9999/erina → AgentCommands de2d87c22（未 push）。
