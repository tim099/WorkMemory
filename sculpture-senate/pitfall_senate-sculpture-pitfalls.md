---
id: pitfall_senate-sculpture-pitfalls
topic: sculpture-senate
title: 雕刻搬家踩過的坑
type: pitfall
status: active
created_at: 2026-10-02
created_by: calli
links: []
related_docs: []
---

- GLFW 只准主執行緒建視窗 ⇒ GUI 頁的背景工作不能自己開 GL context ⇒ 頁面一律 spawn CLI 出圖。
- `./build.sh` 遇到 Tim 的 Senate 視窗開著：`publish/senate.exe` 被鎖 ⇒ publish 失敗，**而 build 已經先停掉 main／tavern 兩顆 Server、失敗後不拉回** ⇒ 先 `tasklist | grep senate.exe` 確認視窗關了再 build；萬一失敗，`publish/senate.exe server start --detach --id main|tavern` 拉回。
- Git Bash 的 `grep -c $'\r'` 數 CRLF 會回 0（量具問題）⇒ 行尾一律用 python 讀 bytes 數 `\r\n`。
- 正交等角圖的「遠大近小」是反向透視錯覺，不是 bug（量平行邊長度相等即可證明）。
- 噗浪共用帳號：lint ⑤ 收「行尾署名」，`op=mentions` 的 SignedBy 只認「最後一行**行首** `—— persona`」（SCP_PlurkOpsSocial.cs:139）⇒ 行尾署名的回應發得出去、對帳永遠算未回。署名請獨立一行。
