---
id: knowhow_headless-async-audio-lab
topic: constitution-broadside
title: 無頭驗非同步結果：DevTools 等 Promise、__game 要 debug=1、環境音量 RMS
type: knowhow
status: active
created_at: 2026-10-17
created_by: unknown
links: []
related_docs: []
---

Blockside 無頭驗聲音／驗頁面裡的非同步結果（TASK-0663，2026-10-17）：

- `--dump-dom` 等不到 OfflineAudioContext：離線錄音在音訊執行緒上跑，虛擬時間（--virtual-time-budget）不等它；不帶虛擬時間則頁面一載入（量到 3 秒）就 dump。⇒ 改開 `--remote-debugging-port`，用 DevTools 協定 `Runtime.evaluate`（`awaitPromise: true`、`returnByValue: true`）等 Promise。src/tests/ambient_lab.py 有一段只用標準函式庫的最小 WebSocket（送一個文字訊框、收可能分好幾個訊框的回應），要再驗別的非同步結果可以照抄。
- 成品的 js 整包包在一個閉包裡：頂層的 function 不在 window 上（`ambLab is not defined`）。⇒ 要從外面呼叫就掛到 `__game`，而 `__game` 只在 `?debug=1` 才建。
- 環境音的音量要量 RMS，不要只聽：第一版浪一條就 −12.8 dB，撞到 master 的限制器（−14 dB）⇒ 晴天微風跟強風都量到 −14.3 dB、分不出來。調完的基準：晴・微風 −29.3、暴風雨 −22.1、暴風雨在艙裡 −29.2 dB（風聲調小 6 dB 之後）。
- 遊戲裡那一份跟離線錄音是兩條路：開戰後讀 `__game.amb()`（風、浪、雨的增益、低通、左右）才是遊戲實際的值；預設鏡頭 23 m 高時浪被高度衰減掉將近一半。
