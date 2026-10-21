---
id: knowhow_acceptance-bearing-20261021
topic: sirius-tidegrave
title: 階段驗收與實體舵支承的接續邊界
type: knowhow
status: active
created_at: 2026-10-21
created_by: Sirius
links: []
related_docs: []
---

2026-10-21接回時以當前事件與VXK做獨立驗收，四階段0541/0545/0548/0550由a60b038閉環。0549發現舵上端沒有明確實體支承；CLI新增固定環座4800與活動短軸/止推肩3528來源格，1cm來源搭顯式scale.1。來源card比例不會再自動轉scale；work create不吃meters_per_voxel，需update。肩須在環座上面承重，不是只在軸頂加帽。

證據正本Sculpture/works/sirius-tidegrave-01m/design/rudder_bearing_20261021/README.md。原舵7428格不變；新件完整360度解析包絡/接觸面，原舵±30度保守區間SAT244呼叫無實碰或未解區間。父代理後驗新舊VXK只差20格原色109水線上通孔，239原模型含mesh、502繩、10帆逐值不變；結構conn1/cavity0/mirror0。遊戲26_spin.js窄測六組舵輸入同軸同角，非完整場景驗收。新VXK SHA9a5780caee1df71a635e0b34b376933ec30e94996467a35e042c3b408959286b，未替換Blockside已匯入資產，下次匯入需再verify/shipcheck。

獨立代理的原報告在雕後完成，carve_guard_bad列20個已空位置，status仍proposal only；不能靜默改成PASS。用凍結前VXK/CLI守門回執加父代理落地讀回另行解釋，保留原報告。沒有把獨立幾何檢查與父代理落地檢查混稱。

0603已由Tim於10-19後台結單。0546最後一格/0540仍保留Tim手動動態驗收，不自動簽全部布動態，不復開舊物理單。歷史main不可盲跑當前成船；新局部件/精確孔用保存來源檔與CLI重現。
