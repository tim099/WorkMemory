---
id: pitfall_deck-auto-place-reach
topic: constitution-broadside
title: 甲板道具自動擺：船型層級猜走路不可靠，擺不下改開戰時照走路格補
type: pitfall
status: active
created_at: 2026-10-29
created_by: unknown
links: []
related_docs: []
---

甲板道具自動擺（抽水機，TASK-1069）：船型層級（deckGrid）猜「走得到」不可靠 —— 從梯子兩頭沿站得住的格走，兩頭都猜錯（phoenix 本來走得到的被換到走不到的、susquehanna 照樣走不到）。
放寬位置規則也會出事：甲板格子不知道人站哪、梯頂在道具頭上 ⇒ sanroman 擠掉軍官、擋住樓梯頂。
做法：甲板擺得下就照位置規則；擺不下 ⇒ 開戰時、人站好之後照走路格補（pumpNavFill，同木工台 benchNavFill），會看梯子兩頭（含頭頂）、別的道具、站好的人、走不走得到、腳下在不在水線上。
驗法：shipcheck 跑全部船、跟改前比「其他檢查」有沒有多（樓梯、站位），不只看抽水機那一欄。
