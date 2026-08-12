---
id: state_day2-v3-flow
topic: manga-adaptation
title: Day2 — 流程走到 v3 並跨 agent 驗證兩次；文件拆三冊；空白樣板落地
type: state
status: superseded
created_at: 2026-08-11
created_by: summit
links: [manga-adaptation/state_day3-002-remote-collab]
related_docs: [ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Workflow.md, ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Author.md, ucl_core:Docs~/zh-Hant/Workflows/Manga_Adaptation_Artist.md, repo:AgentCommands/ArtGallery/Comic/template/README.md]
---

2026-08-11 summit：漫畫化流程從 v1 走到 v3，並跨 agent 驗證兩次。

## 流程版本（今天定的，寫進 DRAWING_MEMO 的戳記機制）
- v1 (08-10) 文字人設 + prompt 文字重建
- v2 (08-11) 三視圖人設（純白零標註）+ 每格掛參考圖 + 正名表
- v3 (08-11) 圖文分離（字幕台詞住 .md、畫面零文字）+ 小問題走原圖微調不重生成

## 文件（UCL_Core）
單檔 31.5KB 拆成三份：Manga_Adaptation_Workflow.md（總文件）/ _Author.md / _Artist.md。
拆冊守則：每條規則只寫在一個地方，其他份用連結指過去。
文件不寫沿革（Tim 拍板，已進 AI_READABILITY_GUIDELINES）——沿革由 git 承載。
commits: 396b8b4 / a225fca / c9d5e70

## 展區
- 空白樣板 AgentCommands/ArtGallery/Comic/template/（七檔：README/NAMING/DRAWING_MEMO/
  分鏡/人設/物件/三視圖範例），可整個複製開新專案。commit 260bb43
- 《桅頂的賭注》002 話 1/9（002_p01 為 v3 的 Stage 3 試畫，已過）；
  上游三張人設卡 + bronze_token_v1 道具卡；Props 三份文字設定已寫。
- 《十八天，同一句話》Sirius 分鏡+作畫，000 話 4/4 完成。

## pending
- 桅頂：masthead / chart-shop / signal-flare 三份 Props 未寫；
  卡戎/鯁/父親文字人設未建；002_p03 起未畫。
- 十八天：001-003 未開；P5（Sirius 以繪師身分出場）定為 003 後的獨立後記。
- 各層未 push：ArtGallery ahead 5 / UCL_Core ahead 1 / Docs~Glossary ahead 1（今晚 commit all）
