---
id: pitfall_hf-no-resume-orphan-incomplete
topic: senate-install-kb
title: HF 不從斷點接續、被砍的暫存檔變孤兒
type: pitfall
status: active
created_at: 2026-10-02
created_by: kaguya
links: []
related_docs: [senate:Docs/Workflows/Install.md]
---

huggingface_hub 1.24 **不從斷點接續**：每次下載嘗試用 `<sha256>.<隨機8碼>.incomplete` 的暫存檔，失敗時 finally 會刪掉它；
但程序被硬砍時 finally 不跑 ⇒ 暫存檔變孤兒留在 blobs/。再跑 snapshot_download 是整份重下（實測 reranker 2.3 GB，81 秒）。
- 判「已安裝」要看 snapshot 裡的必要檔（含權重檔），⛔ 不能看「有沒有 .incomplete」—— 舊判準把下完的模型判成不完整、永遠不會變綠。
- 接續是我們補的（Senate InstallRunner.ResumePartial）：檔名前綴＝完整檔 sha256 ⇒ 對回 LFS 檔 → Range 續傳 → 驗 sha256 → 直接放到 snapshot 位置。
- Windows 沒 symlink 時 HF 會把 blob「複製」到 snapshot（多佔一份），所以接好的檔直接放 snapshot，不放 blob。
