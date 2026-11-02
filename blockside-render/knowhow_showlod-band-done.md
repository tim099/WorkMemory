---
id: knowhow_showlod-band-done
topic: blockside-render
title: 表演船的交接帶做完了（TASK-1104）：抖動互補的接法、SAIL_FS 不能直接加分支
type: knowhow
status: active
created_at: 2026-11-01
created_by: erina
links: [blockside-render/knowhow_showlod-far-done]
related_docs: [D:/Unity/Blocksail/external/Blockside/src/js/60_showlod.js|D:/Unity/Blocksail/external/Blockside/src/SHOW_SPEC.md]
---

**表演船的交接帶做完了（TASK-1104；Blockside dev 30afdc2／f63471f）：抖動互補的接法、帆著色器的坑**（erina 2026-11-01）

**級別**（SHOWLOD.lv）：0 近、1 中、2 遠、3 近↔中交接帶、4 中↔遠交接帶。
- `fd[o]` ＝ t（smoothstep），showlodMid 對 1、4 都回型。
- 抖動在 01_gl.js SHOWD_GLSL（uShowD ＝ (t, ±1)）：＋1 留雜湊 ≥ t 的像素、−1 留 < t 的、0 不管。雜湊同建築 TOWN_FADE／treeDi。

**第 3 級**
- 照近那一級建表。
- drawHulls：showlodBandOut（原模淡出）→ 原模 → showlodBandLow（低模淡入）。
- 模型：rbatColour 跳過 lv ≥ 3 的船（同鏡頭外那樣留組），改由 showlodBandModels 照表一件一件用 P.tri 畫。
- 烘進低模的那幾件淡出：showlodBaked，同 rbatShip 中那一級跳過的那條。
- rbatReplay 同一條。

**第 4 級**
- 照中那一級。
- showlodFadeOut 處理：低模、模型、繩、旗桿。
- 帆：showlodBandSails。
- 看板：showlodFarDraw 帶 (t, −1)。
- 不給海面遮罩：showlodNoMask。

**會咬人**
- ⛔ 不要在 SAIL_FS（P.sail）直接加抖動的分支。加了之後戰鬥模式 render_lab ports_high 106 像素差 1 色階，是編譯器排的指令變了，數學沒變。
  - 怎麼抓到的：--skip-programs 拿掉 P.sail 或 P.tri 任一支就 EXACT，看不出是誰；直接把 SAIL_FS 那一處退回才確定。
  - 現在的做法：P.sail 與循環布 floopFxProg(base, dith) 另外編一支分身（showlodDithFS：原始碼照抄、在 main 開頭插 showCut）。P.vox、P.tri 直接帶 uShowD，仍然 EXACT。
  - 以後改共用的著色器，一定要跑 render_lab 全部 20 組，不能只看自己那個場景。
- 驗互補的量法：
  - 挑四周沒別艘的船（巴斯特爾假清單第 12～15 艘、別艘 ≥ 286 m）。
  - 參考圖：用 n／f 讓「只有這艘」換級（別艘三種設定都同一級）。
  - 整張讀畫布、逐像素分成「等於前一級／等於後一級／都不等」。都不等的是陰影、玻璃、倒影。
  - 遠↔中太遠看不到 ⇒ 把 f 拉到 150 m 量；只算對照組（同設定畫兩次）穩定的像素。
- 城鎮反向對照：牆鐘雜訊（樹擺、水面）讓「逐像素相同」做不到。用「關 ×6 之間穩定的像素」上，看「開 ×6 每一次都不同」的有幾個（0 ＝ 沒有系統性的差）。
- 量測頁：`__game.showlod()` 的 stat 是同一個物件，連讀好幾格要先 JSON 複製（又踩一次）。
- 夜裡才編的著色器（船窗）要在夜裡進一次城驗：m1104/setup_tod.sh 把 iframe 的 tod 改成 22。
- 別人 reimport 沒重烘 ⇒ 那一型靜靜退回原模；合併前跑 `admin.py showlod --check`。import／reimport 現在自己會烘。

**量具**：session 55fb05f7 scratchpad m1104/
- 交接帶互補：bandpix.js（近↔中）、farbandpix.js（中↔遠）。
- revpix.js：城鎮反向對照。
- 截圖：bandshot.sh、farbandshot.sh。
- compose2.py：手機用的對照圖。
