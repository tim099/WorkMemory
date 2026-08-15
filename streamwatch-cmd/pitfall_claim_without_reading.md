---
id: pitfall_claim_without_reading
topic: streamwatch-cmd
title: 回傳檔的宣稱要有讀數撐著；探針要走正式路徑
type: pitfall
status: active
created_at: 2026-08-15
created_by: summit
links: []
related_docs: [ucl_core:Docs~/zh-Hant/Plan/Plan_StreamWatch_Cmd.md]
---

本主題重複出現的同一族坑，三條判準（每條都有本案的血證）：

## ① 回傳檔的宣稱一律要有讀數撐著

印**比較結果**（`窗口尾端 18:29:51 ≤ 水位 18:29:52 ✅`），不印**意圖**（「窗口尾端夾在這裡」）。
🩸 血證：`--before-mtime` 因雞生蛋完全沒生效，而回傳檔那一行**照樣印得一模一樣** ——
沒做卻報告做了，而回傳檔是事後唯一的證據。

推論（本案兩次用到）：讀不到就**明寫讀不到，並往壞的方向解釋**
（「無讀數 ⇒ 當成沒生效看待」），不得沉默通過。

## ② 測試探針必須走正式路徑的同一段程式碼

🩸 血證：手跑 `screenstream_montage.py` 全綠 exit 0，**同一分鐘**同一台機器上
`step=cycle` exit=1 —— 差別只在 Cmd 那條多夾了一個 `--before-mtime`。
**手跑通過從來不是 Cmd 通過。** 所以 `peek`（Tim 要的測試入口）刻意共用取材與對帳程式碼。

## ③ 一條規則會不會被遵守，看照著做走不走得通

🩸 血證：`cycle` 收工印「要補：跑 step=note」，而收工那步關掉 session、
`note` 又要求 active session ⇒ **它指的是一條它自己封死的路**。
比靜默失效難看一級：**指路存在、而且會大聲失敗。**

## ④ 錯誤訊息的通道要當事實查，不要用猜的

🩸 血證：montage 的 `ERROR: 選擇條件下無 frame 命中` 走 **stdout**（不是 stderr），
而 C# 的軟路徑只比對 stderr ⇒ 那條分支**一次都沒被執行過**，
且它是**開場常態**（STT 落後 ~29s，start 後第一輪必中）。
⇒ 判別外部工具的狀態時，stdout/stderr **兩條都比對**；blocked 訊息也要把 stdout 帶出來，
否則回傳檔只印得出 `exit=1;` 後面一片空白。

跨工作通用的版本已收在個人 lessons；這裡留的是**本主題會再撞到**的形狀。
