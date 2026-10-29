---
id: knowhow_flagloop-prebake
topic: blockside-render
title: 循環布的預烘與播放（TASK-1102）：ships/flagloop.js、指紋重烘、成品不准 fetch
type: knowhow
status: active
created_at: 2026-10-29
created_by: erina
links: []
related_docs: []
---

**循環布的預烘與播放（TASK-1102，Blockside dev 0fba364）**（erina 2026-10-29）

在哪：
- js/11_rig_loop.js：兩行標記之間是「烘」那一段，放常數和 floopMake／floopStep／floopWork…。
  - src/flagloop_bake.cjs 把這一段原樣拿到 Node 跑，寫出 ships/flagloop.js（window.BS_FLOOP ＝ { fp, b64 }，deflate-raw 的 int16＋跟上一格的差）。
  - 遊戲裡備援的 Worker 也是拼這幾支 → 兩邊結果一樣。
- 指紋 ＝「烘」那一段＋TOWN_FLAG_DRAG／IT 的 sha1。admin.py build、build --code 最後叫 flagloop_ensure：指紋變了才重烘（Node 約 40 秒）。ships_sync 不刪 flagloop.js。
- 播：
  - 船：帆緩衝多一份第 6 號屬性（每頂點 4 個 float）。drawSails 把有 ow.fx 的船排到最後，用 P.sail／P.dsail 的變體畫（01_gl.js SAIL_VS／DSAIL_VS 換頂點那一段）。
  - 建築旗：floopTownDraw，一次 instanced。

會咬人：
- 成品不准 fetch（VoxelKit/vxk_inline.py「零外部依賴」）⇒ 資料檔照船檔的載法：`<script>`，前綴是 BS_SHELL.ships。解壓用 VK.b64ToBytes＋VK.inflateRaw。
- Blocksail 的打包清單（packaging/electron/tools/copy_game.cjs PICK）本來就整個 external/Blockside/ships 抄 ⇒ 新的資料檔放 ships/ 就跟著走。
- 用 A/B 頁測的時候，external/Blockside/ships 是共用工作區（沒有新檔）⇒ serve_ab.py 用環境變數 FLOOP_JS 把那一支導到 worktree 的那一份，不要動共用工作區。
- 收尖的長旗 30 m/s 阻尼要 0.93 才收斂。逐點檢查接縫：門檻 0.05 mm 時戰鬥旗 4 m/s 有 1 點超過 ⇒ 改 0.01 mm、最多 12 圈。
- 重排程式時，Worker 原始碼字串裡也有 `function floopUpload() {}` ⇒ 照字找位置切到了字串裡、整段重複。找函式要對 `() {` 換行 那種寫法。
- 讀數 __game.floop()：
  - { tails } 每面旗 10 秒內尾端位移；{ ramp } 風速漸增；{ pairs } 同型兩兩比；{ yards: i } 綁在帆桁的點對帆桁的模型；{ control } 原本頻率硬接的反向對照。
  - town().ms.flags：建築旗那一段每格 CPU。
- 換手（表演船變真的）那一格旗從作品形狀重新算，沒接；Tim 要的話再做。
