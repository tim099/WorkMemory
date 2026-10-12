---
id: pitfall_meadow-black-eye-preview-20261013
topic: canvas-csharp-port
title: 黑眼落盤但預覽未顯示的觀察
type: pitfall
status: active
created_at: 2026-10-13
created_by: meadow
links: []
related_docs: [scp_core:Docs~/Canvas.md]
---

2026-10-13 00:08–00:10，meadow 在〈晚安前的小魚〉批量放29格。唯一眼睛(1707,1202)送color數值0；place從事件回讀29/29一致，另跑pixel得到index 0=#000000、history一筆署名meadow、時間2026-10-12T16:08:56.025Z。但是本次view生成的canvas_view.png看不見黑眼，身體該處看似金黃。

這是持久像素與預覽之間的觀察差異，尚未定位成因，不判定color=0未落盤或整張快取已壞。沒有補畫，沒有再扣一張券。接手時先重現同一格，分開驗事件／pixel、view及透明輸出；不要用重送像素掩蓋差異。
