---
id: pitfall_webgl-d3d11-shader-silent
topic: sculpt-frigate
title: WebGL2 著色器在 Windows D3D11 編譯失敗、無頭截圖全對
type: pitfall
status: active
created_at: 2026-10-12
created_by: basecamp
links: []
related_docs: []
---

WebGL2 的片段著色器在 Windows 真瀏覽器（ANGLE → D3D11）編譯失敗，而無頭 Chrome（SwiftShader 軟體算繪）照常 ⇒ 截圖全對、真機上那支程式整個不畫，不會讓頁面停，只在主控台留一行「Error compiling dynamic pixel executable」。

- 實際踩到的寫法：巢狀三元裡包 vec3(sign(x),0,0)（int 字面值）。改 if／else 就好。二分法：開一個新的 webgl2 context，把可疑片段一段一段編、drawArraysInstanced 後讀 getError。
- ⚠ 驗收別只看 getError：程式沒建起來時它也是 0。要讀回畫面（readPixels）數該有的顏色（例：暖色像素數）。
- ⚠ 內建瀏覽器窗格隱藏時 requestAnimationFrame 不觸發 ⇒「點播放時間不走」是量具問題；播放路徑改用無頭 Chrome＋DevTools 協定（Input.dispatchMouseEvent）量。
- ⚠ 無頭的 --virtual-time-budget 在 sandbox iframe 裡等不到非同步解壓 ⇒ 截到「沒有船」不代表壞；用真實時間等再截。
- 另一個：著色器一行裡放 // 註解再接宣告，整行後面都被註解掉（分層開關第一次就這樣壞）。
