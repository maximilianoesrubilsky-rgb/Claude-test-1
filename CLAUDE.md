# Naval war sim — project debrief

`naval_war_sim_3d.html` is a single self-contained file (no network loads): a DCS/Tacview-style 3D naval
engagement simulator. It grew out of a claude.ai chat ("Israeli-Iranian naval war game", Sept 10–17 2026)
and a Cowork session (3D/Tacview view, Sept 17–30). Read this before changing anything.

## How the user works
- Credits are tight. Be fast and targeted: grep for the class/weapon key, edit, verify, ship. No long exploration.
- But it must WORK. Every past round that "fixed" something without verifying end-to-end came back
  (carrier squadron edits reverting took 4 rounds). Find the root cause, then prove it (headless Playwright run,
  render the model, count what fired).
- Realism is the bar: real ship fits, real missile speeds/ranges, real doctrine. Hypotheticals are labelled as such.
- They send reference photos for models. Match the photo: layout along the hull, mast shapes, launcher counts.
- Short answers. Say what changed and what was verified.

## Fixed facts the user set (do not change without asking)
- Israel: 4 Sa'ar 6, 5 Reshef (replacing 5 of the Sa'ar 4.5s), 3 Sa'ar 4.5, Sa'ar 5s, patrol craft.
- Sa'ar 6: 16 Barak-8, 40 C-Dome, 16 Gabriel Mk5 (4 quad launchers). NO Harpoon. Model per the NavalAnalyses
  model photo: 76 mm Super Rapid, 40-cell C-Dome VLS forward, integrated MF-STAR mast, Gabriel well amidships,
  2 x 8-cell Barak-8 (the only rear cells) on the aft roof, enclosed aft tower with twin SATCOM domes, hangar +
  flight deck. Fine detail lives in kSaar6(): Typhoons, Deseavers, ESM ring, lattice topmast with IRST and
  COMINT/DF, EO directors, RHIBs, torpedo doors, flight-deck nets. VLS parts must come after the houses they
  stand on (yr:'a'), Barak-8 first so they are VLS sections 0 and 1.
- Gabriel = the Mk5 (newest, beyond Blue Spear): 400 km, weaving terminal.
- Dolphin I/II: Popeye Turbo SLCM, conventional (not nuclear). INS Drakon: 6 VLS SLBMs (hypothetical),
  stays far back. Dakar class: 8 SLBMs (hypothetical); per the Covert Shores render: long low sail with a rounded
  vertical leading edge and sloping tail, no sail planes, casing, capped bow planes, larger under-casing, X rudders,
  ducted 7-blade propeller. Tubes in the sail (Drakon 6, Dakar 8) whose hatches open at launch (SUB_VLS).
- Drakon/Dakar SLBM = "LORA-ER": stretched two-stage Naval LORA, 9.6 m, 0.9 m booster, 2,500 km, spd 2.7.
  Launch: hatch opens, underwater ejection, broach at 1.25 s, ignition 1.6 s (engine holds it via EJECT like the
  YJ-21's cold launch), then the standard 'bm' arc. Sound = the YJ-21 profile. Boat rides at 35 m while it has SLBMs.
- No nuclear weapons anywhere in the sim (no Trident). Ohio = SSGN (buildOhio): 24 tubes, 2 lock-out + 22 missile,
  smooth turtleback, no DDS; fits via SUB_FITS: 154 Tomahawk, 66 CPS (hypothetical), mixed, or 22 conventional
  Trident D5 (key TRD — CTM is taken by the CTM-290 land missile). No nuclear warheads anywhere.
  In naval world (US), land modes, and the add menu. Tube hatches open at launch (SUB_VLS per-tube counts).
- Aircraft: F-35I Adir (not F-35A) with 6 Sky Sting + 2 Python-5; F-15I with Air LORA. Turkey: TF-2000 with
  Siper Block 2 / ESSM Block 2 / Hisar-RF, MUGEM carrier (30 aircraft), 10 KAAN with ramjet BVR missile.
- World mode: realistic theatres per matchup (US vs China in the Philippine Sea, etc.), per-ship loadout editor
  with missile variants, carrier squadrons with roles and launch order, AWACS extending detection.

## Code map (grep these)
- `CLS` ship classes, `SUBC` subs, `SAM` / `ASM` / `GUN` weapons, `WCLS` world catalogue, `NAVY` rosters.
- `buildAddable()` — the add-unit menu and each unit's default loadout.
- Ship 3D layouts: the parts table keyed by class (`S6:[['gun',...` ), plus per-class extras in `if(_cls==='S6')`.
  Parts: gun, vls (cols/rows/p/base), can (ty, n, pairs, step), house, mast (tower/pyr/quad), dome, ciws, hangar.
- `VSEC` = VLS sections per class (which cell block each weapon launches from).
- Subs: `SAILSPEC`, `XPLANE_SUB`, `buildSubMesh()`, `LEN_SUB`.

## Verifying
Check models in the sim's own renderer (render3D with CAM set on the unit), not only a custom preview — the
preview's painter sorting shows glitches the game doesn't.
Headless Chromium via Playwright (`NODE_PATH=$(npm root -g)`): load the file, check `pageerror`, click `#start`,
and for models call `shipMesh(cls,1)` / `buildSubMesh(cls,1)` and draw them to a canvas to compare with the photos.
