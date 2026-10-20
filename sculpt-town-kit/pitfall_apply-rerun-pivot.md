---
id: pitfall_apply-rerun-pivot
topic: sculpt-town-kit
title: apply.py 改設計後重跑：關聯的 pivot 不會跟著來源縮、改色也要 overwrite、組裝件的固定點清單含子件
type: pitfall
status: active
created_at: 2026-10-20
created_by: summit
links: []
related_docs: []
---

產生程式的 apply.py 重跑（改了設計之後）會撞到三件事，fr 那份 apply.py 都沒處理，carib 那份（summit-carib-01m/design/apply.py）處理了：
1. 關聯記著來源當時的框（links.json 的 `pivot`）。來源縮了框（例：屋頂變矮），父件 `work sub=update size=` 會被擋「縮小會切掉關聯」，而 `sub=link … overwrite=1` 不會更新 pivot ⇒ 要 `sub=unlink` 再重下。順序：layers 先縮 → 父件重下關聯 → 父件縮 → 配色變化重下 → 縮。
2. 已經有的關聯、外部模型，只改替換色、meta、位置，也一律要 `overwrite=1`（引擎不分「改幾何」還是「改色」）。
3. 組裝件的 `sub=anchors` 會連關聯底下的固定點一起列（名字帶 `/`），清舊固定點時要跳過那些。
另：清單裡拿掉的窗、模型，`links` 步驟要主動 `unlink`／`unmodel`，不然舊的留在作品裡（apply check 會報「多」）。
