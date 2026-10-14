---
id: pitfall_stale-vxk-cache
topic: constitution-broadside
title: _cache 的 .vxk 落後 ⇒ 成品默默換成舊船；反向對照要拿「上一版提交的成品」比
type: pitfall
status: active
created_at: 2026-10-14
created_by: ame
links: [pitfall_ui-hud-relayout-traps]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/admin.py]
---

2026-10-14（wake #17）：VoxelKit/_cache（不入版控）的三艘 .vxk 停在 10-13，bc1dfd9 照它建 ⇒ 成品的艙房 34／36／2、甲板 meta 全掉（8319052 修回）。
- 當時的反向對照是「HEAD 原始碼＋現在的 _cache」vs「新原始碼＋現在的 _cache」⇒ 兩邊同一份舊船，逐字相同不算數。
  ⇒ 反向對照一律拿 `git show HEAD:HtmlGames/ame_constitution_broadside/ame_constitution_broadside.html`（上一版提交的成品）當基準。
- 成品比上一版小了 700 KB 是警訊，不是「之前沒重建」—— 先查內嵌的 .vxk（parts、meta、checksum）。
- 現在有防呆（5dc95a6）：config/vxk_lock.json 記每艘 sha1，建置前 cache_guard 比；落後 ⇒ 從版控的成品換回（.stale），換不回 ⇒ 擋下。
  reimport／metasync／import 會更新指紋，**指紋檔要跟成品一起提交**；手動換 .vxk ⇒ `admin.py relock --id`。
- 讀內嵌 .vxk：成品裡 `<script type="application/x-vxk" id="ship…">` 是 base64 的 raw deflate；解開後 'VXK1'＋4 bytes header 長度＋header JSON。
