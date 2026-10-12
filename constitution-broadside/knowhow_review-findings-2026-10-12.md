---
id: knowhow_review-findings-2026-10-12
topic: constitution-broadside
title: 審查確認的 32 項（位置、問題、修法；行號照 54be17d）
type: knowhow
status: active
created_at: 2026-10-12
created_by: ame
links: [constitution-broadside/knowhow_next-batch-plan]
related_docs: [ArtGallery/HtmlGames/_build/ame_constitution_broadside/game.js]
---

2026-10-12 對抗式審查（5 個面向 × 每項 2 位反駁者）確認的 32 項；行號是 ArtGallery `54be17d` 的 game.js。嚴重度 high／medium／low。

1. [high][gun-service] game.js:944 —— Released crew go straight back to the gun they just left, so survivors never gather at a gun that can fire, and they keep shuffling between its spots
   修法：In the fill pass, skip guns with n < crewMin unless the remaining unassigned free crew can bring them up to crewMin. Exclude the gun a crew member was just released from. Don't create a new walk when the chosen gun is the crew member's current gun.
2. [high][requirements-ux] game.js:241 —— Sheets and tacks keep floating in midair after their sail drops (fore mast falls), because each pair only depends on the other
   修法：Record which ropes end at each sail's clew or tack (sheets/tacks, e.g. by name prefix `<sail>.` or a free end within a few voxels of the sail's vertices). Kill them in dropSail and when the sail moves to a piece (or move them with it). Alternatively, when par links form a cycle with no voxel or model anchor, mark the ropes as dependent on the sail.
3. [medium][casualties] game.js:539 —— Explosive shell that hits a model (carronade/wheel/chase) explodes without the '爆炸' casualty radius
   修法：Add `hurt(sh, L, 13, 30, null, '爆炸');` in the `b.ammo === 1` model-hit branch, before `b.alive = false`.
4. [medium][casualties] game.js:739 —— Jib/staysails still float for up to ~0.5 s after the fore mast detaches: spawnPiece skips the sail when its stay was killed in the same call
   修法：In the sails loop, before the `!inA && !inP` early return, drop the sail when any of its par ropes is dead and not in `moved`, for example: `if (s.par.some(function (j) { return sh.ropes[j].dead && !moved.has(j); })) { dropSail(sh, s); return; }`. Also call `moved.add(j)` for par-ropes carried at line 733.
5. [medium][casualties] game.js:752 —— crush() is checked on a segment that starts at the break and points up, so a mast that breaks above deck never crushes anyone
   修法：Test crew against the area the mast sweeps or lands on, not the airborne segment. For example, project horizontally from the break: from the pivot's deck-level foot along normalize([r.x,0,r.z]) for 2|r| at deck height. Or sample the current mast line every hinge step and kill crew it passes within 1.5 m of.
6. [medium][casualties] game.js:933 —— Survivors never regroup: two or more guns each below crewMin stay unmanned forever even when the crew could man one
   修法：Pool crew from all below-min guns of a deck: if their total is at least crewMin, release every below-min group except the largest (or the best gun), even when no gun is at min. In free.forEach, use a decrementing count of unassigned free crew instead of free.length. In the toMin pass, prefer guns with n>0 over n===0.
7. [medium][determinism-perf] game.js:545 —— Every 4th ball substep runs hurt() over all crew, even when the ball is far from the hull. This is about 30% of sim time and allocates heavily in the hot loop
   修法：Before calling hurt for '砲彈', skip when L is outside a per-ship crew AABB expanded by rKill. Compute that box in survey(); at minimum use the grid bounds. Results stay identical because rnd() is only consumed when dist < rKill. In hurt(), compute dx/dy/dz inline instead of sub([..]) and len() so the per-crew allocations go away.
8. [medium][determinism-perf] game.js:941 —— reassign() sends released crew straight back to the gun it just released, so short-handed guns never consolidate
   修法：Keep a set of guns released in this pass and exclude them in both assignment passes. Alternatively, in the toMin=false pass also skip guns with n===0 when free.length < g.crewMin.
9. [medium][gun-service] game.js:883 —— Chase-gun crew are kept apart from the long guns on the same gun deck; once the chase gun is gone its 6 crew stand idle
   修法：Derive c.deck from the deck height (or map chase→'long' here, since its crew z matches the gun deck) instead of from the gun type.
10. [medium][gun-service] game.js:932 —— Two short-handed guns never merge: release only happens when some other gun is between minimum and full
   修法：Let a short-handed gun merge into another short-handed gun when their combined crew reaches crewMin. Release the gun with fewer crew and send them to the one with more.
11. [medium][gun-service] game.js:945 —— toMin pass checks the size of the whole free list, not how many free crew are left, so a group big enough for one gun gets split across empty guns
   修法：Track how many free crew are still unassigned and allow an n===0 gun only when remaining >= crewMin. Better: assign crew to each gun in a batch until it reaches crewMin before starting the next gun.
12. [medium][gun-service] game.js:951 —— Reassignment can send crew to a spot whose deck has been shot away; they fall on arrival and are counted as drowned
   修法：Before choosing a spot, re-check it against sh.grid with standZ/countBox and skip spots that no longer have a floor. If no valid spot is left, use standNear on sh.grid.
13. [medium][gun-service] game.js:953 —— spot.who is never cleared when crew are released or moved, so a few cycles leave all of a gun's spots held by 2 crew
   修法：When c.gun changes (released, gun destroyed, reassigned), clear the spot whose who === c on the old gun. A spot also counts as free when who.gun !== this gun.
14. [medium][render-art] game.js:1030 —— Placeholder DEAD frame is painted almost entirely outside its own atlas cell (into the WOUNDED cell), so dead crew are invisible and wounded crew show a corpse
   修法：Keep the lying figure inside the cell, e.g. g.translate(28, -8); g.rotate(-Math.PI/2); g.scale(0.48, 0.48) (120*s <= 58 px wide, bottom at y <= 0). Or clip each cell: g.save(); g.beginPath(); g.rect(ox, oy, CELL_W, CELL_H); g.clip() ... g.restore().
15. [medium][requirements-ux] game.js:648 —— Hull hardness has no effect on chain shot: slab() always removes the same voxels
   修法：Scale slab() with hardness too: shrink the half-width (10.5+2.5*hash) and depth (d range) by about HARDK^-0.25, or stop removing voxels once the accumulated cost times HARDK exceeds the ball's energy budget.
16. [medium][requirements-ux] game.js:932 —— Crew does not combine onto fewer guns when every under-staffed gun has no partly-manned gun to join
   修法：Plan per deck type: total = number of able gun crew of that type. Choose k = floor(total / crewMin) guns to keep, preferring the most-manned ones. Release every gun outside that set and send its crew to the kept guns, filling each to crewMin first and then to crewFull.
17. [low][casualties] game.js:743 —— Crew thrown off a falling mast get velocity along the hinge axis, perpendicular to the direction the mast falls
   修法：Use the fall direction: fall(sh, c, mul(norm(imp), 2)), using the horizontal impulse computed just above, optionally plus a small upward component.
18. [low][casualties] game.js:829 —— Powder monkey killed or wounded while running keeps moving=true, skips the survey floor check forever, and floats
   修法：Clear c.moving in woundCrew, killCrew and fall, or change the survey skip to `(c.st === 0 && (c.walk || c.moving))`.
19. [low][casualties] game.js:988 —— Falling crew pass through lower decks and the hull, so any drop through a deck hole is counted as drowned
   修法：While st===4, transform c.fw into the ship's local frame each step. If a solid voxel is under it, land there (st 2, or wounded, with cause '墜落') and store pos back in grid coordinates. Count '落水' only when y<0 is reached outside the hull.
20. [low][determinism-perf] game.js:1428 —— ?hard= / ?flood= URL values are overwritten by the slider's min/max/step, so a ?demo URL silently runs a different config
   修法：Use the same min/max/step for the URL clamp and the input attributes. Alternatively, keep the URL value authoritative: skip reading back from the slider until the user moves it, or set the input's min/max from the code constants.
21. [low][gun-service] game.js:904 —— Helmsmen are never placed: both helm spots fall inside the wheel model's reserved footprint
   修法：Put the helm spots just outside the reserved footprint (for example x = bb[0]-3 or bb[3]+3), or exclude the wheel from the reserved area when placing helm crew.
22. [low][gun-service] game.js:952 —— Fallback destination is inside the gun barrel/carriage; 砲-S02 has only 6 spots, so it hits this early
   修法：Find the fallback with standNear(sh.grid, …) around the breech, or cap assignment at spots.length instead of crewFull.
23. [low][gun-service] game.js:1416 —— A ship that has struck its colours can still fire
   修法：Return false from fireGun, and gate broadside/fireOne, when sh.struck is true.
24. [low][render-art] game.js:68 —— Carronade, chase-gun and wheel meshes are lit with abs(dot(n,sun)) and flat ambient, so their shadow side and underside get full sunlight
   修法：In buildMeshes (line 637), compute the normal as cross(d-a, b-a) (or swap b/d) so normals point outward. Draw models with one-sided lighting matching VOXEL_FS (max(dot) and mix(uGround,uSky,n.y*0.5+0.5)), e.g. via a uTwoSided uniform that sails keep at 1.
25. [low][render-art] game.js:596 —— A model dropped by survey() has no matrix yet, so it disappears for one rendered frame
   修法：Initialise M: M0 in the FLY.push object (or call stepFly(f, 0) once when pushing).
26. [low][render-art] game.js:1053 —— Placeholder props overflow into the neighbouring atlas cells (rammer and musket/sword stubs appear in other frames)
   修法：Clip drawFigure to its cell (save, rect(ox,oy,CELL_W,CELL_H), clip, restore), or shorten the props so x <= 30 from the cell centre.
27. [low][render-art] game.js:1083 —— teamRecolor matches key colours by exact value: partial-alpha and in-between key pixels stay magenta
   修法：Classify by hue with a tolerance (R close to B, G small, R > 96) and pick the nearest of 255/192/128, or map luminance continuously onto the team ramp. Decode without premultiply or colour conversion, e.g. createImageBitmap(im, {premultiplyAlpha:'none', colorSpaceConversion:'none'}).
28. [low][render-art] game.js:1115 —— hud.png is accepted at any size and stretched into a fixed 8x2 grid, while the readout claims 'HUD：美術'
   修法：Require im.width === 4 * im.height (8 square columns x 2 rows). Otherwise push the reason into why and leave HUD_ART null.
29. [low][render-art] game.js:1125 —— Alpha-tested crew billboards sample a full mip chain: legs drop below the 0.5 cutoff at distance, so figures render as torsos floating above the deck
   修法：For CREW_TEX, cap gl.TEXTURE_MAX_LEVEL at 2-3, or rescale alpha per mip level (preserve coverage), or use alpha-to-coverage, or lower the discard threshold for distant LODs.
30. [low][requirements-ux] game.js:160 —— ?flood / ?hard handling disagrees with the sliders: bad input silently turns sinking off, and the code's clamp limits are never used
   修法：Fall back to the defaults (0.35 / 2) on NaN, and clamp to the same limits as the sliders (0..2, 0.5..5), or widen the slider min/max to match.
31. [low][requirements-ux] game.js:879 —— Gun 砲-S02 reloads 22% slower from the start with no casualties, because crewFull is fixed at 8 but only 6 standing spots are found
   修法：Set gn.crewFull = gn.spots.length (keep CREW_FULL as the upper limit), or report or fix the missing spots when the crew is built.
32. [low][requirements-ux] game.js:1416 —— A ship that has struck its flag keeps firing: the player can still fire after a defeat, and the AI keeps firing at a struck player
   修法：Also block player fire on me.struck (Space and F), and have ai() stop firing when foe.struck is set (or when OVER, outside sandbox).
