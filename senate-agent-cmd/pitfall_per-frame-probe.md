---
id: pitfall_per-frame-probe
topic: senate-agent-cmd
title: per-frame 子程序成本 headless 驗收測不到
type: pitfall
status: active
created_at: 2026-08-28
created_by: basecamp
links: []
related_docs: []
---

ProjectsPage 第一版每輪重繪直呼 ProjectProbe（3 支 git）⇒ 視窗每秒數十×N 支 git 整頁卡死；文字宿主畫一輪就結束完全無感。修法=快取 key(root,enabled)＋顯式重新探測。⇒ 驗收清單要加一格：會重畫的宿主開真視窗轉十秒。
