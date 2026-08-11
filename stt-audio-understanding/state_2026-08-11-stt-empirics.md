---
id: state_2026-08-11-stt-empirics
topic: stt-audio-understanding
title: state 2026-08-11：prompt 三格 A/B 完成、audio_sec 已 ship、四條過濾待實作
type: state
status: active
created_at: 2026-08-11
created_by: Sirius
links: [workmem:screenstream-recording]
related_docs: []
---

# 現況（2026-08-11 23:00，Sirius）

## 已 ship
- **UCL_Core `1c9568c`**（單層，**父層仍指舊 hash，同事 pull 拿不到**）：
  - `stt_prompt` 改為 UCL_ScreenStreamPage 可編輯欄位（含字數警示、生效時機提示）
  - 錄影狀態三塊併成單行狀態條（紅/綠底色保留，細節收摺頁）
  - `write_stt_chunk` 加 `audio_sec`（常駐 worker 與 montage --stt-live 兩路都餵）
- Library `chapter 0001–0005` 全片心得（`status=finished`），逐筆來源與觀看限制都在章內。

## 待辦（依優先序）
1. **per-segment 有序子序列過濾**（擋 prompt 回吐）—— 約 5 行，未實作。
   ⚠ python 模組已載入 daemon 記憶體 → 要 **toggle STT 開關 off→on** 才生效，成本掉 1 chunk。
2. **跨 segment 重複過濾**（`ん`×15／`2`×13 逃過現有三層，因為 compression_ratio 是每段各算）。
3. **幻聽片語黑名單**（`ご視聴ありがとうございました` 等三變體；它們逃過是因為模型**很有信心**）。
4. **prompt 改寫成漢字混寫自然句**再測一輪（同時驗名詞覆蓋與漂移是否雙贏）。
5. 離線 A/B：`audio_transcribe.py file some.wav` 已支援吃 WAV，**但怎麼留下 WAV 還沒解**
   （daemon 的 `recording_*` grep 不到音訊軌處理）。
6. **不建議**動 `no_speech_max`/`logprob_min`（basecamp 的 0.685 血證：誤殺 5 段真對白）——
   那兩個旋鈕要等分離層做好才有意義。

## 顯存/模型（Tim 問過）
- 磁碟不是問題：`medium.pt` 實測 1.5G，large-v3 約 3GB。
- **顯存才是**：whisper README 表為 medium≈5GB／large≈10GB；本機實測總量 12282 MiB、
  已用 3229 MiB（Unity+daemon）→ **10+3.2 > 12.0，極可能 OOM。**
- large-v3 另有已知回歸：**非語音段幻聽比 v2 更嚴重**（正是本場主要失效）→ 不是純升級。
- 想要 large-v3 又不吃顯存 → **換 runtime**：faster-whisper（CTranslate2）`int8_float16`
  約 3–4.5GB，比現在的 medium 還省、快 2–4 倍。本機實測 **faster_whisper/ctranslate2 都沒安裝**。
  這條是 basecamp 見叢掛十天的舊債。

## 未提交
- `AgentCommands` 200+ 筆資料層（五章 Library、bookshelf、letters、整晚酒館訊息、audit）
- `ArtGallery`（nested repo）：`sirius_the_naming_stakes.svg` + Diary（自由時間手寫 SVG）
