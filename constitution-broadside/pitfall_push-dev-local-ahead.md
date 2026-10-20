---
id: pitfall_push-dev-local-ahead
topic: constitution-broadside
title: 推 dev 前本機 dev 要等於遠端；管線會吞掉 ff 失敗
type: pitfall
status: active
created_at: 2026-10-18
created_by: unknown
links: []
related_docs: []
---

推 dev 之前，先確認共用工作區（D:/Unity/Valhalla/Blockside）本機 dev ＝ 遠端 dev/dev。別人合完、收了施工場卻還沒推的那一筆，會躺在本機 dev 上；這時 ff 會失敗。

🩸 2026-10-18（apex-one）：`git merge --ff-only <分支> 2>&1 | tail -1 && … && git push dev heads/dev:dev` —— 管線的結束碼是 tail 的，ff 失敗沒擋住，結果只把 erina 還沒推的 c3f1ffc 推上去、我的那筆反而沒上。

手勢：
- 整串用 `set -eo pipefail`，⛔ 不把 git merge／ff 接 `| tail`、`| grep`。
- ff 之前：`test "$(git rev-parse refs/heads/dev)" = "$(git rev-parse refs/remotes/dev/dev)"`；不一樣 ⇒ `git log dev/dev..dev` 看是誰的，先問、或等他推（我等了 10 分鐘）。
- 推之前：`git log refs/remotes/dev/dev..refs/heads/dev` 只該有自己的；有別人的就在酒館講。
- 另一個會擋建置的：別人換了 vxk_lock 卻沒把新的 .vxk 放進共用 _cache ⇒ `admin.py cache --all`（從作品重新匯出、跟指紋比）先試，對不上的那艘不替別人 relock，問作者或 Tim。
