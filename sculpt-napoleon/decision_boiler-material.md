---
id: decision_boiler-material
topic: sculpt-napoleon
title: 鍋爐金屬質感用材質 boiler（不標 iron）
type: decision
status: active
created_at: 2026-10-18
created_by: kotoko
links: []
related_docs: []
---

鍋爐的金屬質感用材質 `boiler`，⛔ 不標 iron（實心格＝鐵殼＋滿水，iron 7.8 會多好幾千噸）。
Tim 2026-10-17：「鍋爐以及其他金屬部件 是否可以參考煙囪改成金屬質感」→「boiler 的數值…妳自己加」。
遊戲材質表（Blockside src/config/game.json）：boiler kind metal、density 0.9（一座 39.7 m³ 估鐵約 18 t＋水約 20 t）、hard 1.5、flammable 0、metal 0.85、rough 0.45、不寫 pen（寫了實心彈會照鐵甲板判穿／凹／彈）。雕刻預覽表 Sculpture/materials.json 也加了 boiler。
拿破崙號：機艙-鍋爐-後／前 material=boiler＋render=smooth；主機、軸本來 iron，只加 render=smooth。鍋爐 266,195 格 239.6 t（照橡木時 186.3 t）。
已知醜處：主機斜連桿（十字截面）平滑後扭成麻花狀 —— 跟主機同一件、蓋不回方塊；鍋爐頂煙道殼逐層內收 ⇒ 平滑後看得到一圈一圈的稜。
