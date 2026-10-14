---
id: decision_blockside-pack-fingerprint
topic: constitution-broadside
title: 遊戲獨立成 Blockside、成品預設壓縮、.vxk 內容指紋與換機器 cache
type: decision
status: active
created_at: 2026-10-15
created_by: unknown
links: []
related_docs: []
---

## 遊戲獨立成 Blockside、成品預設壓縮、.vxk 內容指紋（2026-10-15，Tim 拍板）

- **住址**：Valhalla 的 `Blockside/` submodule（github tim099/Blockside，Pages：https://tim099.github.io/Blockside/）。`src/`（原 ArtGallery `HtmlGames/_build/ame_constitution_broadside/`）、`VoxelKit/`（複製一份；`_cache/` 不入版控）、根目錄 `index.html`（成品＝Pages 首頁）、`admin.html`／`admin.cmd`、`sandbox.html`。D:\Unity\Blockside 那份工作副本不用。
- **為什麼搬**：ArtGallery 的 Pages 每次上傳整個畫廊 2.45 GB（> 1 GB 上限），會隨機逾時 —— 1ed2d40 那次就沒部署上去，線上停在 9.5 MB 的舊成品、下載慢到看起來像卡在載入中。
- **成品預設壓縮**（TASK-0583）：`build` 預設把模型網格收成池子（同一份只存一次、頂點去重＋差值）、寫之前照遊戲的讀法還原跟 _cache 原檔比；`build --raw` 原格式；`verify` 對帳。網格才是大頭（格子只佔一成多）。
- **內容指紋**（TASK-0586）：vxk_lock.json 存 `c:<sha1>` ＝ header 拿掉 exported_at（照鍵排序）＋格子段 —— 同一件作品換機器重新匯出指紋一樣（量過四艘）。整檔 sha1 做不到（匯出時間不同）。
- **換機器**：`python -I src/admin.py cache` 補缺的（從雕刻作品重新匯出、跟指紋比，對得上才放）；`cache --all` 每艘都比 ＝ 比對來源。對不上 ⇒ 不蓋、留 `<船>.vxk.source`、說哪一艘；要用新的走 `reimport --id`（會改戰局，要跟 Tim 說）。壓縮版成品解不回原檔，所以不再從成品解回。
- **找現有元件**：`senate cmd sculpture --arg op=work --arg sub=components [--arg kind=gun]`（造船規格 §2.1）。
