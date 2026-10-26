---
id: decision_fort-is-fixed-ship
topic: blocksail-fort-combat
title: 堡壘 ＝ fixed 的船：照船匯入、fixed 跳過浮力與移動
type: decision
status: active
created_at: 2026-10-26
created_by: summit
links: []
related_docs: [D:/Unity/Blocksail/external/Blockside/src/README.md]
---

堡壘不是另一種單位，是一艘 `fixed` 的船（Blockside 58338aa，TASK-0954）。
- 碉堡作品本來就有船式的砲元件（kind gun）、甲板、彈藥庫 meta ⇒ `admin.py import --work <作品> --id fort_xx`，ships[] 加 `"fixed": true`、`zw0: 1`、geo、crew。
- sh.fixed 跳過：stepFloat（進水、下沉、傾斜、浪、上浮）、sailShip、船對船碰撞、擱淺、棧橋、水格、水的房間、砲口進水、survey 的浮力與動力。地面高 ＝ sh.gy（實驗場 0）。
- 選單只列 HULL_SHIPS；網址 ?blue=…&red=fort_en 照樣進得來。
- 失守 ＝ 砲全啞 10 秒（固定的不看選單降旗規則）。
- 現在匯了兩座：fort_en（磚）、fort_carib（毛石）。en-battery 也有砲元件、還沒匯；carib 砲台兩種、岩堡、加拉塔堡沒有砲元件（0955 要補或寫明不參戰）。
- 0955 要接的地方：sh.gy 照地形、crew 照城鎮帳那一棟的 staff、敵我（引擎只有 blue／red）、碰撞（現在船穿得過去）。
