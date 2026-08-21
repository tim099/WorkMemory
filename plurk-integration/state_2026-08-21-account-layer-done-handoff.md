---
id: state_2026-08-21-account-layer-done-handoff
topic: plurk-integration
title: 帳號層完成並交接 basecamp；OAuth 唯讀已打通；lint 四條硬約束與 A/B 待拍板
type: state
status: superseded
created_at: 2026-08-21
created_by: summit
links: [plurk-integration/state_2026-08-21-all-shipped]
related_docs: [ucl_core:Docs~/{lang}/Plan/Plan_Plurk_Bot.md, ucl_core:Docs~/{lang}/UCL_EditorPage/UCL_PlurkAdminPage.md, ucl_core:Docs~/{lang}/Workflows/Plurk_Posting_Workflow.md, ucl_core:Docs~/{lang}/Workflows/Secret_Manager_Workflow.md]
---

# 現況（2026-08-21 收工，已交接 @basecamp）

## 帳號層：**完成且驗過**

- `UCL_PlurkAccounts.Resolve(persona)` 三段：persona profile 的 `plurk_account`
  → registry `plurk_accounts.json` 的 `SharedSecretId` → `unset`。**回值帶 `Source`**。
- 個人／共用**由 Source 推導**，不另存 kind 欄位（少一個會漂的地方）。
  `Source == shared-default` ⇒ `RequiresSignature`（共用帳號末行署名必填，Tim 08-16 硬規則）。
- 後台頁 `UCL_PlurkAdminPage`（住 `Editor/`，因為要用 `UCL_SecretScanner`；
  ⚠ 組件引用單向 `UCL_CoreEditor → UCL_Core`，放錯邊是 CS0246）。
- 頁面「🔑 產生憑證」：四欄直接產 `.enc`，**明文不落地**；覆蓋要顯式勾選；成功後清空欄位。
- 共用帳號已裝好：`plurk_shared`，四欄到齊、`PlainExists=True`，
  `Resolve` 各 persona 回「共用帳號（plurk_shared）」。

## OAuth：**basecamp 08-21 打通（唯讀）**

`POST /APP/Users/me → 200`，`id=18174200 nick_name=valhalla_valkyries`。
新增 `Tools~/AgentCommands/plurk.py`（只有 `resolve` 不連網 ＋ `whoami` 唯讀，**刻意沒有發文 op**）。

🩸 我 Plan §5 標的「官方 API 頁 403、agent 讀不到」**是誤框**：
那是 **Cloudflare `error code: 1010`** 擋 `Python-urllib` 預設 UA，加顯式 User-Agent 就 200。
⇒ 判準（basecamp 立的）：**簽章錯／端點不存在／WAF 擋，三種失敗都是 4xx，長得一樣。**
⚠ 環境坑：Git Bash 會把 `--endpoint /APP/...` 改寫成 `C:/Program Files/Git/APP/...`，
**不報錯、只是打去別的地方** ⇒ 要帶 `MSYS_NO_PATHCONV=1`。

## 未做（Plan §6）

`lint` / `preview` / `post` 三期都沒動。發文現況仍是 Tim 手動貼。

### lint 的四條硬約束（交接時談定，別重新發明）

1. **拿 08-07 與 08-11 兩篇真的出事的文案當測試樣本** —— 擋不下它們就是沒做完。
   乾淨樣本驗證等於沒驗（樣本不會走進錯誤分支）。
2. **`post` payload 必帶公開度，沒指定就擋**。逐篇公開度是現有控制項
   （所有人／自訂／本人），bot 只會發公開噗＝把它拿掉，而那種消失不報錯。
   ⇒ 危險不只是失去控制項，是把預設從「每篇都要決定」變成「不決定就公開」。
3. **表情提示按 persona 分** —— `[emoN]` 表是各人的品味（我 6 個、basecamp 8 個），
   做一張共用表就是用我的尺量別人的文案。
4. ⭐ **lint 不能給「可以發」的綠燈。** 公開判準是「有人被傷到嗎」，機器判不了 ⇒
   lint 只驗形式（字數／斷行／署名／註記殘留），輸出要明說**本檢查不含公開度審查**，
   否則過 lint 會被讀成過審查。

### 待 Tim 拍板

**共用帳號長文拆則：A（兩則獨立噗，原拍板）還是 B（第二則走回應）？**
我的理由：共用時間軸多 persona 共用 ⇒ `(1/2)` 與 `(2/2)` 中間會被別人插隊，
標記救不了閱讀順序，而且一次吃兩格洗板額度 —— 這是**個人帳號沒有的成本**。
⇒ 建議共用走 B、個人走 A。不管哪個，**署名每一則都要**（B 的回應也會被單獨看到）。

## 相關硬數字

- 單筆上限 **300 字**（Tim 08-21）—— 不是 360。`[emoN]` 是字面 token 會吃額度，
  且 **Plurk Paste 不渲染表情** ⇒「超過只是換形態」在帶表情的文案上不成立。
- 憑證存 `<data_root>/Secret/plurk_shared.enc`（private submodule）；
  資料夾名由 `<data_root>/secrets_config.json` 的 `SecretsDir` 決定，**不是寫死**。
