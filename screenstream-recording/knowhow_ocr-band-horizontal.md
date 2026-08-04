---
id: knowhow_ocr-band-horizontal
topic: screenstream-recording
title: 字幕帶水平可調（OcrBand）— 兩端都改，且水平欄位缺席＝滿寬
type: knowhow
status: active
created_at: 2026-08-04
created_by: unknown
links: []
related_docs: []
---

2026-08-04 Tim 需求：字幕 OCR 帶原本只能調垂直（底部原點 y + 高度），水平固定滿寬。

**抽成共用型別 OcrBand**（主帶與額外區域同型別、同 UI DrawBandFields、同序列化）。原本主帶是兩個散落 float、額外區域是 Vector2 借位（x 存 y_bottom、y 存 h —— 欄名與內容不同義）。

**水平用「中心＋寬」不是「左緣＋寬」**：字幕對齊畫面中央，調寬時人要的是往中間收；左緣制會邊收邊往右推。

**必須兩端都改**（只改 C# = 設定寫了沒人讀）：
- C#: ocr_x_center_pct / ocr_w_pct + 區域的 x_center_pct / w_pct；預覽框水平也畫；**3-way merge baseline 要含水平兩欄**（SerializeRegions 少寫的話，只改寬度會被判成「Tim 沒動過」→ 下次 reload 被磁碟值靜默蓋掉）
- Python: normalize_regions 回 4-tuple、regions_from_config 讀水平兩欄、_crop_band_to_array 真的水平裁、唯一解包點（subtitle_ocr.py 的 for 迴圈）跟上

**向後相容**：水平欄位缺席一律 0.5 / 1.0（滿寬）＝改動前行為。舊 config / 舊 cache / 只傳 (y,h) 的舊 caller 全部不變。

**實戰驗收**（比合成圖測試有力）：2026-08-04 Steins;Gate 01 陪看 6 輪，regions=(0.0391, 0.15, 0.5, 0.854) 裁掉左右各 ~7% 排除 bilibili 浮水印與播放器邊緣，六輪 cache 100% 命中、0 幀遺失、沒切掉任何有效字幕（含當集最關鍵那句對白）。

commit: UCL_Core 02460c8
