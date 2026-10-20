---
id: knowhow_report-mailto-discord
topic: constitution-broadside
title: 錯誤回報：mailto 2083 字上限、預設郵件程式壞掉 ⇒ 改送 Discord webhook
type: knowhow
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

錯誤回報寄不出去的兩個原因（2026-10-18 排查，01_gl.js）：
- mailto 網址太長：中文 encodeURIComponent 一字 9 字元，一封帶遊戲狀態的回報 2575 字；Windows 交給郵件程式的網址超過約 2083 字就不開、也不報錯 ⇒ 超過 1900 字信裡換短版、完整內容先複製到剪貼簿。https 新分頁沒有這個限制（Chrome 約 2 MB）。
- 那台電腦 mailto 的預設程式（HKCU UrlAssociations\mailto 的 ProgId）查不到註冊 ⇒ 什麼都不跳。
最後的解：一鍵送 Discord webhook（XMLHttpRequest multipart：payload_json＋report.txt＋截圖）。Discord 的 webhook 會回 CORS 允許 persona9999.github.io（file:// 也收得到）⇒ 讀得到成功與否。
webhook 在 game.json 存成 x1:…（XOR＋base64url 的混淆，鑰匙在 admin.py 與 01_gl.js 各一份，⛔ 改一邊另一邊解不開）；明文只從設定後台或 `admin.py hook`（標準輸入）進來。
