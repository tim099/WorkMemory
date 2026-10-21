---
id: knowhow_r3-sail-rag-islands
topic: blockside-render
title: 帆破洞毛邊（R3）與斷開那一塊不畫：資料、開關、量法
type: knowhow
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

帆破洞的毛邊（R3，TASK-0791，dev 133fade）與斷開那一塊不畫（dev 60480e9），都在 11_rig_gpu.js、01_gl.js。

- 毛邊：靜的那份多 3 個 float（SB_SW 12，第 5 號屬性 aH ＝ 離洞幾圈、u、v）。離洞幾圈照物理的洞（put 的 th，不含 sailIsland 藏的）算；u、v ＝ 這一面攤得最開的兩個軸除以平均邊長（以細格為單位）。著色器 RAG_GLSL：「吃多深」照慢、快雜訊，離洞越遠越不吃 ⇒ 不會在布中間開小孔（第一版把雜訊加在離洞幾圈上，就是蕾絲）。布片外緣吃 0.35 倍，每一點都在外緣的小布片不套。開關 ?sailrag=0。
- 斷開那一塊：燒穿與毛邊只在著色器裡，物理還連著 ⇒ 被圍起來的布浮在空中。看得到的點照「兩點都看得到的邊」分塊；物理的那一團要照模擬的約束（c.T.sim.ea／eb、c.brk，細格照 T.mi 對到粒子），⛔ 不能照還在的三角形 —— 撕開一圈 tv 會被算成另一團、漏抓。每團只留點數最多的一塊，其他併進不畫的點（索引疊成一點）。只在 tv 內容、c.brk、s.burn、燒穿點數換了才重算。開關 ?sailisl=0；?debug=1 看 __rbat.stat.islRuns／islHide／islFaces。
- 量測：render_lab 換了假時鐘 ⇒ 頁面裡的 performance.now 量不出時間（islMs 0），要看 ab 的 tick／render CPU。造一塊斷開的布最便宜：hook 裡把一圈點的 s.burn.a 設成很早（燒穿，只影響畫）或一圈 c.tv ＝ 1（不動 brk），同一個成品 ?sailisl=0 對預設。
- 對拍：?rigchk=1 的 sailFill 跟 sailBuf 共用 sailWalk ⇒ 毛邊、斷開的點都要在 sailWalk／put 裡算，對拍才會逐位元相同（rigMiss 0）。
