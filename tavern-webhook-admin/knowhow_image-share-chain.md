---
id: knowhow_image-share-chain
topic: tavern-webhook-admin
title: 繪圖自動分享鏈：2D/3D→酒館帶圖→mirror multipart 上 Discord
type: knowhow
status: active
created_at: 2026-08-20
created_by: basecamp
links: []
related_docs: [Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_TavernImageShare.cs, Assets/Plugins/UCL_Core/UCL_Core_Scripts/EditorCore/UCL_AgentCommands/ChatTavern/UCL_DiscordMirrorDaemon.cs, Assets/Plugins/UCL_Core/Tools~/AgentCommands/canvas.py]
---

繪圖自動分享鏈（Tim 2026-08-20 拍板，未 commit）：2D place／3D 落子 → 自動發預覽圖進酒館（帶 refs）→ mirror 附件分支自動上 Discord。

## 四塊
1. `UCL_TavernImageShare`（ChatTavern/，新）：in-process `Cmd_Tavern.ExecuteAsync` 發帶 refs 訊息 —— 繼承完整管線（presence／mention／mirror／頭像）。refs＝repo 相對路徑；失敗只回 false 不拋（分享不汙染主動作）。
2. `UCL_DiscordMirrorDaemon`：訊息帶本地圖片 refs → 首段改走 `StartPostMultipart` 上傳檔案（png/jpg/jpeg/gif/webp、≤24MB、每則 ≤4 張、跳過出聲）；無圖走原 StartPost 逐位元不變。inbound 訊息 meta.source=discord 本來就被 mirror 跳過 ⇒ 無回音。
3. `Cmd_Sculpture`：box/carve/stamp 成功且 actual>0 → 引擎渲全景 view → 複製 `Sculpture/previews/share_*.png`（獨立檔名 —— _last_view 是會被蓋的共用檔）→ PostAsync（tag=sculpt-share）。`--arg share=false` 可關。
4. `canvas.py place`：落點 bbox＋8px 邊裁圖、NEAREST 放大到最長邊 ~512 → `Canvas/previews/share_*.png` → **run_cmd submit（fire-and-forget）＋ --lane share**（tag=canvas-share）。`--no-share` 可關。
   ⚠ 兩個真坑的修法都在這：(a) 等 Tavern Cmd 跑完＝Editor 等 python、python 等 Editor 自鎖 ⇒ submit 不 wait；(b) 主 lane 被代跑中的 Cmd 佔住 ⇒ ensure_idle 空等 ⇒ 用 share 子 lane（獨立 queue＋running-lock）。

previews/ 兩目錄進 AgentCommands/.gitignore（臨時渲染，真相源是 events）。

## 驗過（實彈）
- 2D：place 1 px → #12823 tag=canvas-share refs 掛上、預覽圖正確（火堆×16）。
- 3D：box 1 voxel → #12824 tag=sculpt-share refs 掛上。
- Discord：兩則 uuid（6b96b3/aa1d32）multipart HTTP 200 × 2 webhooks，log 無附件跳過警告。
- recompile 0 errors；canvas.py py_compile OK。

## 邊界
- Discord 端「圖看得到」只有 Tim 能目測（HTTP 200＋回 msg id 是 Discord 收下 multipart 的證據）。
- Sculpture 分享渲的是全景 view（無 region）—— 大場景時哪天嫌吵再改 region 裁切。
- glossary 對分享訊息照常附掛（成果分享是對話不是指令，刻意不 opt-out）。
