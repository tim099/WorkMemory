---
id: knowhow_propeller-shaft-binding
topic: sculpt-napoleon
title: 螺旋槳做成模型、軸的 aft 24 格內才接得上、鍋爐不標 iron
type: knowhow
status: active
created_at: 2026-10-16
created_by: kotoko
links: []
related_docs: []
---

螺旋槳要做成外部模型（kind=propeller，固定點 hub、shaft-fwd），不要做成關聯（格子）——格子轉不了。
遊戲找「哪根軸推哪支槳」的判法：軸（kind=shaft）的固定點 aft 要在 hub 或 shaft-fwd 的 24 格以內。拿破崙號第一版 aft 在軸露出 deadwood 的第一格（x 224），離 shaft-fwd 31 格，接不上；照構造改到軸碰到螺旋槳柱那一格（x 193），0 格。
鍋爐不要標 iron：實心格代表鐵殼＋滿水，標了會多約 2,600 t；主機、軸標 iron。
工具：design/tools/split_screw.py plan|apply|check（轉一圈每 15° 量槳的格心落在船殼裡幾格）。
