---
id: pitfall_shared-worktree-mixed-build
topic: constitution-broadside
title: Blockside 共用工作區：在工作區建的成品會包進別人施工到一半的改動（Tim 測到的「卡死」）
type: pitfall
status: active
created_at: 2026-10-16
created_by: unknown
links: []
related_docs: []
---

## 什麼時候會咬
Blockside 的工作區是好幾個人共用的。有人在工作區跑 `admin.py build`（或 import／reimport，結尾會自動 build），index.html 就會把**當下工作區所有人沒提交的改動**一起建進去，包括別人施工到一半的。Tim 打開它測，就是在測一份混合版。

2026-10-16 實例：
- Sirius 接潮墓號時在工作區建了成品，順手包進 meadow 改到一半的 24_armor.js。
- 那一版 `shatter(…, a)` 的參數 a 被函式裡的 `var a = rnd()…` 蓋成數字 ⇒ armRemove 讀 `a.grid.set` 丟例外。
- 例外丟在 frame() 中間，requestAnimationFrame 排在最後一行 ⇒ 整場停住、UI 還能按。Tim 以為是卡死。

## 怎麼查
- `git hash-object index.html` 對 `git rev-parse HEAD:index.html`：不一樣 ⇒ 工作區那份不是 HEAD 的成品。
- 無頭重現：errtest.js 塞進成品，8 個種子各跑 240 秒，抓第一個例外的 stack。

## 怎麼避
- 要給 Tim 測的版本：用 HEAD 建在鏡像。用 `git archive HEAD src VoxelKit index.html README.md admin.html | tar -x` 解出來、再複製 _cache。⛔ 不要用 `git --work-tree=<鏡像> checkout`：它會改寫真正的 index 的 stat 資訊，工作區會冒出一排「M 但沒差異」的檔。
- 推到 dev 的就是 git 裡的成品 ⇒ 那一份一定乾淨。
- 自己的段落要合進別人也在改的檔：在工作區現場套補丁（前後文對不上就停）。暫存區放鏡像的版本，用 hash-object -w --path ＋ update-index。

## 順帶：var 會蓋掉同名參數
JS 函式裡 `var a` 跟參數 a 是同一個變數。在一支早就有 `var a` 的長函式加參數，名字要先 grep 過函式本體。
