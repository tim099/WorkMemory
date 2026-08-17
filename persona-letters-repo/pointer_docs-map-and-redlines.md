---
id: pointer_docs-map-and-redlines
topic: persona-letters-repo
title: 文件地圖 + 起手三條驗收指令 + 三條紅線（不 push / 代裝不代寫 / 沒 hooks 別設 hooksPath）
type: pointer
status: active
created_at: 2026-08-17
created_by: summit
links: [persona-identity-layers/state_20260804]
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Persona_Letters_Submodule_Workflow.md, ucl_core:Docs~/zh-Hant/Workflows/Commit_Workflow.md, AgentCommands/ChatTavern/baton/letters/summit/.gitignore]
---

## 知識本體在文件，這裡只給 key → 位置

| key | 去哪 |
|---|---|
| **四階段全流程**（A 落檔 / B 遠端·讓位 / C 掛 submodule / D clone-local 配置） | `ucl_core:Docs~/zh-Hant/Workflows/Persona_Letters_Submodule_Workflow.md` |
| 護欄三行的**理由全文**（為什麼 `_wake_brief.md` 不能入版控） | 直接抄 `letters/summit/.gitignore`（含逐行註解，是目前的參考實作） |
| `.gitattributes` 釘 hook 行尾的理由 | 同上目錄的 `.gitattributes` |
| 提交規範（雙 persona / 絕對路徑 / `--message-file`） | `ucl_core:Docs~/zh-Hant/Workflows/Commit_Workflow.md` |
| 公開 vs 私密的判準（sketchbook 可公開、隱私才進 sealed） | Workflow A1 段（Tim 2026-08-05 拍板） |

## 起手三條指令（代他人操刀時）

```bash
# 1. 護欄是不是真的跟參考實作一致 —— 比內容不比 md5（CRLF 會讓 md5 全紅而真差異為 0）
diff --strip-trailing-cr <ref>/.gitignore <target>/.gitignore

# 2. 三條規則逐一確認命中（不是「檔名不在 staged 清單」就算過）
git -C <target> check-ignore -v _wake_brief.md _ding_brief.md sealed/x.md

# 3. 掃 staged blob 全文（掃內容不掃檔名）
git -C <target> diff --cached | grep -nE "\b[0-9a-f]{32}\b"
git -C <target> diff --cached | grep -nE "[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.(com|net|org|tw)"
```

## 三條紅線（做之前先知道）

1. **agent 不 push。** 信件庫 origin 是公開 GitHub —— push 是**發佈**行為不是存檔行為，交回 Tim。
2. **代裝不代寫。** `README.md` / `_constitution.md` / `tools/` / `sealed/` 是 persona 本人的東西。
   代他人配置時只放**護欄**與**落檔**，不替人寫自我介紹或憲法。
3. **repo 沒有 `tools/githooks/` 就不要設 `core.hooksPath`。**
   指向不存在路徑的 hook 設定，在 `git config` 裡跟「防線已上線」長得**一模一樣**，而它一次都不會生效。
   略過時要在 commit 訊息**明寫**「本次不設，等 X 後補」—— 讓下一個人知道那是決定不是遺漏。
