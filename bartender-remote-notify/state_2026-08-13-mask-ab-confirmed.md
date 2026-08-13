---
id: state_2026-08-13-mask-ab-confirmed
topic: bartender-remote-notify
title: 遮蔽假說 A/B 實測確認 + debug 面上線 + 遷移陷阱
type: state
status: active
created_at: 2026-08-13
created_by: basecamp
links: []
related_docs: []
---

## A/B 實測結果（2026-08-13 22:25-22:27，basecamp + summit 兩人在線）
遮蔽假說**已由現場實測確認**，不再只是讀 code 推導。

同一 persona / 同一 daemon / interval 10s / 冷卻 120s：
- ① @ 落在 `tavern`（seq 15076，> 水位 15075）→ 新 @ **1** → 權重 10 →
  22:25:30 酒保**實際戳到**（游標 (1010,972) → 左鍵 → 逐字 `/ucl-ding` → Enter ×3）
- ② @ 落在新房 `notify-mask-ab`（seq **1**，水位 15076）→ 新 @ **0**、標記整房遮蔽 →
  通知池 0、**沒被戳**。淘汰理由印的是 `all_acked` 而非 `cooldown`
  （冷卻 22:27:30 已到期 ⇒ 排除混淆因子）

⇒ 側房 @ 的靜默是**永久**的，與時間、冷卻、retry 全部無關。

## 已上線的除錯面
- `run_cmd.py run Bartender --arg op=notify_scan`（純觀測，不寫 state / 不發告警 / 不戳人）
  → 逐人判定 + 逐房 inbox 分解 + 整房遮蔽標記，落 `ChatTavern/_last_op.md`
- `bartender/remote_notify_trace.jsonl`（append-only；已驗證會落 notified 事件）
- AdminPage 逐人判定痕跡（遮蔽紅字）＋ 掃描時間戳
- ⚠ AdminPage 的掃描已改純觀測（`applyStateChanges:false`）——
  舊行為是每 2 秒重繪就推一次所有人的已讀水位，看板自己會改掉正在追的訊號

## 尚未修（等 Tim 拍板）
水位改 per-room。⚠ 遷移陷阱：舊單值水位要當成「tavern 房的水位」，其餘房從 0 起算會讓側房
一次湧出上百筆歷史 @（summit 的 trpg-yachiyo 有 34 筆、basecamp 25+4 筆）把人戳爆 ——
建議遷移時把既有房一律視為已讀（取各房當前 max seq），只有之後的新 @ 才算新。
