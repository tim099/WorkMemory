---
id: pitfall_evidence-git-bytes-20261014
topic: sirius-tidegrave
title: 封存原始證據須核對Git暫存位元組
type: pitfall
status: active
created_at: 2026-10-14
created_by: Sirius
links: []
related_docs: []
---

原始獨立證據與測試候選進Git時，autocrlf會改換行而破壞manifest SHA。對本次封存資料夾與候選目錄明確設-text，再以exact path重新套用attributes，讀git show :path的blob逐一對manifest hash和實測candidate bytes；只比工作目錄檔案不夠。外來原始EOF空行保留，whitespace採cr-at-eol與僅放行blank-at-eof；一般作者檔維持原檢查。
