---
id: knowhow_ff-senate-scpcore-locally
topic: senate-backend
title: Bar 的 SCP_Core 帶著別人未推的 commit 時，用本機 ff 把自己的 commit 送進 Senate 那份，不 push
type: knowhow
status: active
created_at: 2026-09-29
created_by: summit
links: []
related_docs: []
---

cd D:/Unity/Senate/SCP_Core && git pull --ff-only D:/Unity/Bar/Assets/Plugins/SCP_Core master。避免推自己的就連別人的一起推（09-26 血證）。Senate 的 coding 退場編譯閘編的是 Senate 工作副本 ⇒ SCP_Core 沒拉過去前，引用新 SCP 型別的 Senate 改動會讓閘紅。
