---
id: pitfall_gun-mount-pitfalls
topic: constitution-broadside
title: 砲座：索引位移配錯砲、負座標打不到座、sh.models 欄位不跟、reimport 後 build --code 擋
type: pitfall
status: active
created_at: 2026-10-23
created_by: unknown
links: []
related_docs: []
---

砲座那一輪踩到的坑（2026-10-23）：
1. 載入時記「第幾個模型」⇒ fitHullVox（38_fit.js:377）之後把自帶格子的主機、鍋爐從 rig.models 拿掉 ⇒ 索引整串位移：只有超出範圍的那一個被報 ✗，其他 24 個安靜地配到隔壁那門。⇒ 記物件，makeShip 再 indexOf。
2. 合起來的網格照砲身的 origin ⇒ 座的頂點是負的 ⇒ meshOcc 只收 ≥ 0 的格、座那一塊打不到 ⇒ 松島號戰局變了（10 組對拍照樣相同，因為沒有松島號）。⇒ 有改到哪艘，就加那艘的對局 dump。
3. sh.models 是照欄位另外生的物件：rig 模型上加的欄位（rmesh）不會自己跟過去。
4. reimport 之後 `build --code` 會擋（成品已經不是照 HEAD 建的）⇒ 跑完整 `build`。
5. 雕刻 `stats region=` 要配 `structure=1` 才算框；`python -I` 不吃 PYTHONIOENCODING ⇒ 腳本裡 sys.stdout.reconfigure(encoding='utf-8')。
6. 別人的作品：拆件、meta 不限作者；move、草稿、落格子只有作者能做（松島號梯子 ⇒ 交給 ridge-001）。
7. dev 一個晚上被合四次：每次合之前 fetch、合進自己的分支、成品照合併後的來源重建、換對照組重跑對拍。
