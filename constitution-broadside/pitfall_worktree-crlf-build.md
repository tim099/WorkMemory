---
id: pitfall_worktree-crlf-build
topic: constitution-broadside
title: Blockside 新 worktree 是 CRLF：game.json 內嵌進成品 ⇒ 同一個 HEAD 建出不同的 index.html
type: pitfall
status: active
created_at: 2026-10-16
created_by: ame
links: [constitution-broadside/decision_blockside-coding-scope]
related_docs: []
---

## 什麼時候會咬
從 Blockside 開新的 worktree（`git worktree add`）⇒ 這台機器 core.autocrlf=true，新 checkout 的檔是 CRLF；主工作區的檔是 LF（建立後沒再被 checkout 過）。
`admin.py build` 把 src/config/game.json 原樣 base64 內嵌進 index.html ⇒ 同一個 HEAD，在 worktree 建出來的成品跟 HEAD 那份不一樣（只差 game-config 那一段的換行），提交就會多一筆假差異。

## 怎麼查
`git diff --ignore-cr-at-eol --stat index.html` 不是空的 ⇒ 抽出 `id="game-config"` 那段 base64 解開來比，看是不是 `\r\n`。

## 怎麼避
開完 worktree、build 之前，把 src/js/*.js、src/config/game.json、src/*.html 轉成 LF（git 會把它們當成沒改）。
⚠ src/sandbox.html 在版本庫裡本來就是 CRLF ⇒ 轉了會變成真的差異，轉完 `git checkout -- src/sandbox.html` 還原。
轉完先 build 一次，確認 `git diff --quiet index.html`（跟 HEAD 逐位元組相同）再開始改。
