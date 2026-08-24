---
id: decision_rulings-20260824
topic: task-management-system
title: 拍板四題：memory_topic 單值／久未更新用 Task.updated_at／QA 閘不擴大但要出聲／施工順序
type: decision
status: active
created_at: 2026-08-24
created_by: basecamp
links: []
related_docs: []
---

# 拍板（basecamp as PM，2026-08-24）—— 四題

## ① `memory_topic` ＝ **單值字串**（不是陣列）

理由不是「多主題想不到」，是**錨點必須唯一才叫「穩定」**（Tim 的規格用詞）。
陣列會立刻長出第二個決定：`op=show` 該印哪一個主題的內容？優先序又由誰定？
⇒ 而多主題的需求**已經有承載處**：記憶側的 `link`（跨主題關聯，`read --with-links` 一起拉）。
**Task 側保持單值，發散留給記憶側** —— 那一層本來就是為關聯設計的。

## ② 久未更新的判準改用 **Task.updated_at**，並**沿用 `STALE_DAYS`**

原設計是「主題的 `state` 超過 N 天沒更新」。**Tim 拍板進度不進記憶之後，那個基準不存在了。**
⇒ 改成：**未關單的 `updated_at` 超過門檻** ⇒ 晚安印出來。

而門檻**沿用 sweep 的 `STALE_DAYS`（14）**，不另開常數 ——
我原本要另開一個（理由是「語意不同、代價不同」），但那是在進度還在記憶裡的前提下。
現在兩者量的是**同一件事：這張單多久沒動**。
📌 **同一個量就該一個常數；不同的量才需要各自的常數。** 我差點反過來做。

## ③ QA 閘只認 `role=qa` —— 行為不改，但要**出聲**

🩸 現場：TASK-0009 被 commit 直接關成 `done`，而我在那張單上掛的是 `pm` 不是 `qa`
⇒ 閘判「沒有指名 QA ⇒ 沒有人要驗」，直接結。而我一整天都在驗她的交付。

**這格主要是我的疏失**：我沒把自己的角色宣告完整。⇒ 已補 `qa` 並 reopen 回 `in_review`。

拍板（兩件分開）：
- **閘不擴大** —— `pm` 不當 QA 閘。PM 排序、QA 簽名，混在一起會讓「有人管」被讀成「有人驗」。
- **但落差要出聲**（⇒ TASK-0015 加一條）：`op=commit` 要直接 `done` 前，
  若單上參與者有 **dev 以外的角色卻沒有 qa**，印一行
  「本單沒有 QA ⇒ 直接結；若這不是預期，reopen 並補 assign」。
  ⚠ **是警示不是擋** —— 擋會讓真正不需要 QA 的小單無法自動結，而那是設計要的。

## ④ 我先做不依賴 ①② 的部分

`work_memory.py` 的 `archive` 本體 ＋ **歸檔前的 git 守衛**（TASK-0017 前兩條）——
它們不碰 Task 側欄位，可以現在動。
