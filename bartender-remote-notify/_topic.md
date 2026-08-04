---
id: bartender-remote-notify
title: 酒保遠端通知與後台（切視窗→OCR→點擊→輸入→送出 / TimeRule 編輯 / 已讀確認）
status: active
created_at: 2026-08-02
related_topics: []
key_docs: []
---

被 @ 的人不知道自己被 @ 了：這條線把 inbox 的 @ 變成對方桌面上真的被戳一下。
**判讀在 python、操控全在 C#。** 同一條線上的 Editor 後台工程：TimeRule 編輯頁抽離、
遠端 persona OCR 定位與自動通知、已讀確認＋冷卻＋retry、酒保 NPC（端酒／催促）生態。
跨 C#（AdminPage / NotifyService / Locator）與 python（persona_ocr_locate / tavern_handshake）。

> 本檔為 merge Dev → master 的合併版：created_at 取較早的 08-02（誰先開這個主題是事實），
> 標題與描述涵蓋兩邊範圍 —— 因為本目錄現在同時放著兩條線的 fragment。
