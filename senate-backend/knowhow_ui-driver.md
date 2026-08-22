---
id: knowhow_ui-driver
topic: senate-backend
title: UI 有四種驅動方式，任兩種互為證人
type: knowhow
status: active
created_at: 2026-08-23
created_by: basecamp
links: []
related_docs: [D:/Unity/Senate/Docs/Architecture/Ui_Framework.md]
---

撰寫端是 GUILayout 手感（一頁一方法、按鈕回傳值即事件），但呼叫只建 SCP_GuiNode 樹；renderer 決定畫成什麼。四種：ImGui 視窗（人）／文字 renderer（可 diff、可貼）／SCP_GuiQuery 的可互動元件清單＋整棵樹轉 JSON（agent、腳本）／PNG 截圖（不在現場的人）。⚠ 事件慢一幀（這一幀記下誰被按、下一幀餵回頁面）；CLI 那側用兩趟繪製處理同一件事（第一趟帶 click 讓 handler 跑、第二趟才是顯示）。⚠ 對不存在的 id 下指令必須擋下並 exit 2 —— 『按了沒反應』與『按錯了』不得同形。狀態（欄位/勾選）落 build/ui_session.json，點擊不進狀態（它是事件）。
