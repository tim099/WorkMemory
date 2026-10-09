---
id: pitfall_gl-pitfalls-1009
topic: globe-gl
title: GPU 預覽踩過的七格（uniform 型別、跳過判準、GL 狀態、線寬、翻轉…）
type: pitfall
status: active
created_at: 2026-10-09
created_by: unknown
links: []
related_docs: []
---

2026-10-09 踩到的（都在提交前修掉、有讀數）：
1. Silk.NET `gl.Uniform3(loc, int,int,int)` 會挑到 glUniform3i —— 面基底是 List<int>，打在 vec3 上 ＝ InvalidOperation。一律先轉 float。
2. selftest 第一版把任何 TryRender 失敗都判「沒有 GPU ⇒ 跳過」——上面那個 bug 因此長得像跳過。⇒ 只有 context／shader 建不出來（InitError）才算沒有 GPU。
3. 在 ImGui 建樹途中畫 GL：要保存／還原的不只 FBO／viewport／program／VAO／貼圖，還有 ClearColor、DepthMask、CullFace 模式、混色函式與方程式、Pack／Unpack alignment、貼圖單元 2。漏一格 ⇒ 整個 UI 畫歪，不報錯。
4. 疊圖線寬用 max(緯度導數, 經度導數) 四條邊共用 ⇒ 球邊透視壓縮讓另一方向的邊變粗（Tim 截圖抓到）；每條邊要用自己座標的螢幕梯度。面接縫只看 fwidth(face) 會變虛線。
5. GL 慣例貼圖（第 0 列在最下）交給 ImGui 要 uv (0,1)-(1,0)；球面 shader 自己把 gl_FragCoord.y 當由上到下就不用翻。
6. 雕刻框景點只收「至少一面外露」的 voxel：內部 voxel 永遠不是投影極值 ⇒ 結果逐位元相同、點數大減（小木屋 CLI md5 改前改後一樣）。
7. 「沒有天空時背景是漸層」：拿單一背景色數「作品佔幾個像素」會數出全部 —— 要跟一張空場景比。
