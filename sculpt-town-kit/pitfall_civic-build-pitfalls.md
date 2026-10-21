---
id: pitfall_civic-build-pitfalls
topic: sculpt-town-kit
title: 城鎮建築套件：layers 漏列、斜的一格厚就浮空、縮框先重下關聯、布旗不跟固定點（TASK-0772）
type: pitfall
status: active
created_at: 2026-10-21
created_by: unknown
links: []
related_docs: []
---

TASK-0772（2026-10-21，ame）踩到的，城鎮建築套件專屬的坑：

1. **layers 沒列到的層不會落格**：build.py 只把 `B['layers']` 裡的層寫成子作品。火藥庫的屋頂（roof）沒列 ⇒ 引擎 check「缺 1144」。有用 shingle_roof／build_roof 就要列 roof。
2. **斜的東西一格厚就浮空**：
   - 斜船台上的船殼板，每降一格高、每換一格寬就只剩邊碰邊，被算成 44 塊浮空 ⇒ 板厚 2 格。
   - 陽台斜撐一格一格往下也一樣 ⇒ 改直的托梁。
   - 斜的桿、梯子：同一層把上一層的位置也補一格。
3. **框縮小要先重下關聯**：關聯記著來源當時的框（pivot）⇒ 子作品縮了，成品 `update size` 會被「縮小會切掉關聯」擋。先跑 apply 的 links（會 relink），再 create。
4. **固定點挪了，布旗不會跟著**：sails 那一步看到同名就跳過，旗尾的座標還是舊的 ⇒ 先 `work sub=unsail` 再跑 sails。
5. **小屋的簷口壓門框**：門 2.3 m 高，牆只到 2.2–2.4 m ⇒ 簷口落在門楣上撞格。`small_house` 已經改成牆至少 2.7 m。
6. **橫梁要真的架在柱上**：鐘的橫梁沿 x 擺在中間，柱子在四角 ⇒ 整組浮空。從頂板吊一個鐵環下來。
7. **生成器那頭**：
   - 特殊建築在坡上 ⇒ 石基把整塊地塊墊成方的石台（Fort de la Roche 6.3 m）。
   - 爬陡坎的路照直線內插會埋進坎裡（main f8943eb 的「路面沒埋」會 ✗）⇒ 特殊建築的路帶 `ramp`，路面取周圍 2 m 內最高的地形。
   - 把坡度上限一級一級往上加只會讓路繞遠（972 m），還是埋。
