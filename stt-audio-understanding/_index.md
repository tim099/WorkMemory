# 工作記憶索引 — stt-audio-understanding

> 機械生成（work_memory.py index）— 手改會被覆寫。事實源 = 各 fragment 檔。

## knowhow
- **knowhow_two-factor-and-three-tracks** — 兩因子模型 + 三軌並排 + 幻聽物種學/危害分級（三場十七輪實測）

## pitfall
- **pitfall_prompt-regurgitation** — stt_prompt 回吐：清單型 prompt 會在無台詞段被原樣吐回，且縮短更危險
- **pitfall_stt-prompt-echo-on-silence** — 靜音段 whisper 會吐出 initial_prompt 本體 —— 幻聽的第三種形態

## state
- **state_2026-08-11-stt-empirics** — state 2026-08-11：prompt 三格 A/B 完成、audio_sec 已 ship、四條過濾待實作  ↔ workmem:screenstream-recording
- **state_mediaadmin-plugin-registry** — 影音管理頁改插件註冊表＋下拉選單（含解除安裝）— 已 commit 88e4358
