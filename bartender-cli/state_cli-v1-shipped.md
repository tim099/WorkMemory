---
id: state_cli-v1-shipped
topic: bartender-cli
title: v1 上線：help / remote-window on-off / msg 群發，13 條路徑實測過，實體送出零讀數
type: state
status: superseded
created_at: 2026-08-20
created_by: basecamp
links: [bartender-llm, bartender-remote-notify, bartender-cli/state_cli-v2-config-commands]
related_docs: []
---

## 這是什麼

從**聊天酒館發一句 `cmd …`** 去動 Editor。第四種酒保發言來源（前三：keyword trigger／time rule／`@酒保` 點名），也是唯一一種**會改變 Editor 狀態、甚至動別人鍵盤**的。

## 檔案在哪

| 檔 | 角色 |
|---|---|
| `Bartender/UCL_BartenderCliSettings.cs` | 設定（白名單／前綴／確認逾時）＋ pending 狀態 ＋ 兩者的 IO |
| `Bartender/UCL_BartenderCliService.cs` | 指令表、解析、授權、確認、發言 |
| `Bartender/UCL_BartenderDaemon.cs` | 掛點（訊息迴圈新增一支，**排在 mention 與 keyword 之前**） |
| `Bartender/UCL_RemoteNotifyService.cs` | 新增 `DeliverTextTo`（送任意文字給指定在線 persona） |
| `Bartender/UCL_BartenderInlineParser.cs` | `StripAutoAttachedBlocks` 改 public（第二個消費端） |
| `UCL_EditorMenuPages/UCL_BartenderAdminPage.cs` | 「🔧 酒館 CLI」區塊（白名單增刪／總開關／前綴／逾時） |
| `ChatTavern/bartender/cli_settings.json` | 白名單（入版控） |
| `ChatTavern/bartender/cli_state.json` | pending 佇列（**不入版控**，執行期狀態） |

## 拍板與判準（別重新發明）

- **白名單精確比對**（sender_id／sender_name／sender_persona 任一全等，忽略大小寫）——
  刻意**不沿用** keyword trigger 的 liberal substring：那邊猜錯只是多發一則罐頭，
  這裡猜錯是把遙控權給錯人（`Tim` 會連 `Tim2` 一起放行）。
- **空清單＝全部擋光**（fail-closed）。空清單最可能是檔案剛生成或被清掉。
- **二次確認只守「拆掉護欄／對外送出」的方向**：`off` 不問，`on permanent` 與 `msg` 要問。
  反過來會訓練人無腦按 Y，那時確認只是儀式。
- **pending 落磁碟**（domain reload 每次編譯都發生，記憶體裡的會無聲消失）。
- **第二筆需確認的指令擋下不覆蓋**（否則那句 Y 會落在使用者以為的另一個指令上）。
- **執行時用當初那一行重跑**，不是用「y」那一行（否則 `msg` 會送出空訊息）。
- **收件名單在執行時才解析**（確認到執行之間有人上下線）。
- **遠端視窗協作是 msg 的總閘**，沒開直接拒絕、一個都不送。

## 兩隻會實際害人的（已修，別再踩）

1. **詞典污染** —— 指令提到 persona 名時 `Cmd_Glossary` 會把「本回提到的新詞」附到訊息尾端，
   那段變成指令的一部分 ⇒ 群發會把**整本詞典打進對方輸入框並按 Enter**。
   修法走**既有**的 `UCL_BartenderInlineParser.StripAutoAttachedBlocks`，剝除器只留一份。
2. **小寫化吃掉訊息** —— 比對要小寫（Tim 指定），但內容若也走小寫 token，
   `Free Time` 會變 `free time` 而**沒有任何一層報錯**。
   ⇒ CliContext 同時帶「小寫 token」與「原始整行」；原文靠「在原始字串上依序切掉
   prefix／指令名／第一個 arg」取得（把小寫 token 接回去接不回大小寫也接不回換行）。

## 已知債

- `DeliverTextTo` 與 `RunOnceCore` 的送出序列**重複**（九步、三次前景驗證），兩份會漂。
  沒抽共用是因為 `RunOnceCore` 每步都綁 `Finish(pool, chosen, …)` 的通知帳，
  抽出來要一併重整那條**現在唯一在跑**的通知路徑。已寫進註解當標記。

## 驗收狀態

酒館實測 13 條路徑全過（白名單擋／混大小寫／on 與 off 回讀值／confirm Y-N／已有 pending 擋下／
逾期作廢／未知指令／msg 確認回顯訊息原文與在線名單／總閘關著時拒送／英文大小寫保留）。
Tim 本人 23:25 打過 `cmd help` 成功 —— **那是白名單預設條目的第二證人**。

⚠ **實體送出那段（定位→點擊→貼上→Enter）零讀數** —— 當時在線只有 basecamp 與 kiara，
真送會打進 kiara 的視窗並按 Enter，對外且不可逆，不擅自做。

## 下一步（kiara 2026-08-20 給的）

語域漂移的早期判準第 ① 條（**首 3-5 token 的語法骨架**）可以接進同一個
「判 drift ⇒ ok=false ⇒ 退罐頭」的位置 —— 比現有的截斷判定更早。
第 ③ 條（推論當下掛 logit bias）要先量 ollama 的 API 有沒有開 `logit_bias`，**目前零讀數**。
