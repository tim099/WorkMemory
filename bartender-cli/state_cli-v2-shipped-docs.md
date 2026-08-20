---
id: state_cli-v2-shipped-docs
topic: bartender-cli
title: v2 指令設定化落地（d102895/f6ac5b2）：config＋設定頁＋HelpURL 文件，Tim 實測 OK
type: state
status: active
created_at: 2026-08-20
created_by: basecamp
links: [bartender-cli/state_cli-v2-config-commands]
related_docs: []
---

v2：指令設定化（Tim 2026-08-20 拍板，未 commit）。

## 形狀
- 指令表從 hardcode 改為 `ChatTavern/bartender/cli_commands/<id>.json` 一指令一檔；空目錄自動生預設三指令（help / remote-window / msg，行為與 v1 一致）。
- 行為封裝：`IBartenderCliAction`（UCLI_TypeListable＋UCLI_IsEnable）＋ `UCL_BartenderCliActionBase : UnityJsonSerializable`（比照 LY ConditionBase pattern）；config 用 `[SerializeReference] List<IBartenderCliAction> actions` —— 一指令多行為依序執行，任一行為 NeedsConfirm 即整個指令要確認。現有 action：CliAction_Help / RemoteWindow / Msg / PostText（固定文字，自訂指令素材）。
- arg→行為參數 mapping 層刻意沒做（Tim 拍板先不做）。
- 設定頁 `UCL_BartenderCliCommandsPage`（入口：酒保管理 → 🔧 酒館 CLI → 📜 指令設定）：整份 List 交給 DrawObjectData（Tim 指定），顯式存檔（id=檔名，逐 keystroke 自動存會把半成品 id 寫成檔）；SaveAll＝寫清單內每筆＋刪不在清單上的檔（改名靠這條收斂）。
- 寫入層攔截：`UCL_BartenderCliService.LooksLikeCliCommand`（只看 prefix 不驗白名單）；Cmd_Tavern post 判中→打 `tag=cli-cmd`＋`cli_cmd=true`＋跳過 glossary auto-attach（詞典污染從源頭擋，讀取端 Strip 保留防舊訊息）；DiscordInboundDaemon RelayOne 同判準打 tag。

## 動的檔（全 UCL_Core）
- 新：Bartender/UCL_BartenderCliCommandConfig.cs（context/action/config/store）、UCL_EditorMenuPages/UCL_BartenderCliCommandsPage.cs
- 改：Bartender/UCL_BartenderCliService.cs（config 驅動）、ChatTavern/Cmd_Tavern.cs（攔截+tag）、ChatTavern/UCL_DiscordInboundDaemon.cs（tag）、UCL_EditorMenuPages/UCL_BartenderAdminPage.cs（入口鈕）

## 驗過的讀數
- recompile 0 errors；Invoke 實測 LoadAll（3 檔生成）/ BuildHelp（由 config 生成）/ LooksLikeCliCommand("cmd help")=True。
- json 多型 round-trip：remote-window.json 帶 ClassName/ClassData；BuildHelp 第二次載入走 deserialize 路徑正常。
- 端到端：發 `cmd help`（seq 12689）→ meta 帶 tag=cli-cmd + cli_cmd=true、body 無 glossary 附掛 → 酒保回 cli-denied（basecamp 不在白名單，預期）。

## 後續已落地（2026-08-20 同日）
- commit d102895（指令設定化＋折疊＋入口）／f6ac5b2（HelpURL＋文件）。Tim 實測 UI OK 並親自微調（三鈕移 TopBarButtons）。
- 文件已建：UCL_EditorPage/UCL_BartenderCliCommandsPage.md（總覽）＋ API/BartenderCli/CliAction_{RemoteWindow,Msg,PostText}.md，與 code 的 [HelpURL] 雙向對齊；DrawObjectData 讀型別 HelpURLAttribute 自動畫 ? 鈕（零客製 UI）。
- 仍未做：arg→行為參數 mapping 層（拍板先不做）；父層 pointer 未 bump 未 push。
