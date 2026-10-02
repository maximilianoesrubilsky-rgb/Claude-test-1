# Handoff: DCS-style naval/land/air sim — work done in Claude Code (Sept 30 – Oct 2 2026)

Paste this into the "DCS sim 3D simulation" chat and attach the latest `naval_war_sim_3d.html`
(single self-contained file). Repo: maximilianoesrubilsky-rgb/Claude-test-1, branch `claude/youthful-tesla-7wmp4v`.
`CLAUDE.md` in the repo is the full project debrief (fixed facts, code map, how to verify).

## How I work (keep doing this)
Credits are tight, so be fast and targeted, but every change must be verified (headless Playwright runs, renders,
counting what fired). Realism is the bar: real fits, speeds, ranges, doctrine. Hypotheticals are labelled. Answers stay short.
No nuclear weapons anywhere in the sim.

## Models
- **Sa'ar 6**, rebuilt from the NavalAnalyses model photo: 76 mm gun, 40-cell C-Dome VLS forward, MF-STAR mast,
  16 Gabriel Mk5 (4 quads) amidships, 2×8 Barak-8 on the aft roof (the only rear cells), aft tower with twin SATCOM
  domes, hangar and deck. Fine detail: Typhoons, Deseavers, ESM ring, lattice topmast with IRST, EO directors, RHIBs.
  No Harpoon. Gabriel = Mk5 (400 km, weaving terminal).
- **Dakar class** (Covert Shores render): long low sail, X rudders, ducted propeller, 8 sail tubes (hypothetical).
  **INS Drakon** has 6 tubes. The hatches open at launch.
- **LORA-ER SLBM** (hypothetical): stretched two-stage Naval LORA, 9.6 m, 2,500 km. Underwater ejection, broach,
  ignition, then a ballistic arc. Uses the YJ-21 sound.
- **Ohio SSGN**: smooth turtleback, 22 missile tubes + 2 lock-out tubes. Tube hatches open at launch.
  - Loadout editor: tubes per round type. Quick fits: 154 Tomahawk Block Vb, 154 Block Va Maritime Strike,
    84 + 70 mixed, 66 CPS, or 22 conventional Trident D5.
  - The Trident D5 has its own model.

## Weapons
- C-Dome fires from its own VLS cells. Israeli CIWS works. Siper Block 3 added.
- Tomahawk is split into two weapons:
  - **TLAM = Block Vb, land attack only.** It is never fired at ships.
  - **MST = Block Va Maritime Strike, anti-ship.**
  - Loads: Burke 16 TLAM + 8 MST, Ticonderoga 18 + 8, Virginia 8 + 4 (land-air-sea), Ohio in naval mode 154 MST.
  - Target priority: carrier, then air-defence ships, then other big ships.
- **Trident D5 (conventional)**: metre-class accuracy. Theatre SAMs like HQ-9B get only 1/5 of their usual kill
  chance against its ~6 km/s re-entry body. If its target is already sunk, it switches to the nearest live ship.
  Test (22 fired): 8 hits, 4 intercepted, 0 terminal misses (it used to lose 17 to HQ-9).
- F-35A of Japan, Australia and Norway carry JSM land-attack.

## Air AI (land and land-air-sea modes, measured)
- Fighter commits are sticky. New commits happen inside about 130 km, a fighter keeps its target for 60 s before
  switching, and it drops a bandit only after 20 s cold. This stopped the intercept loops.
- Jets remember which side of a SAM site they chose to go around (90 s), so they don't circle SAM envelopes.
- Realistic scrambles: engine start, then taxi, then pairs rolling together, vectored at the raid.
- Every jet has a job:
  - CAP rotation by fewest sorties.
  - Fighter sweeps.
  - 2-ship strikes.
  - Rear-area jets ferry forward.
  - About 94% of blue fighters fly within 4 h.
- No more shaking: turns are roll-rate limited, wingmen hold the lead's heading, station spacing is smoothed.
  Heading jitter went from 479 reversals to ~60 per 30 min.

## Land-air-sea fixes (latest)
- Fleets no longer count enemy land-attack missiles as threats to ships. Chinese groups used to think they were
  out-ranged by Burke Tomahawks and never closed.
- Submarines now get anti-ship orders. Attack boats start on patrol just outside their missile reach of the enemy
  fleet and hunt it; the Ohio stays back. Chinese 093B and Yuan subs now fire their YJ-18s.
- Surface groups close to missile range at about 27 kn.
- The Turkish fleet sails from Aksaz in the Aegean, or from the Black Sea when fighting Russia. Greece–Turkey now fights.
- **New option, "Battle ends: Automatically / When I end it"** (land modes). With "When I end it" the battle runs
  until the **End battle** button is pressed, then shows the after-action report. Tested: ran to 10 h.

## Known and intended
- Israel–Iran fleets are about 2,500 km apart in different seas, so they never meet.
- A group that the enemy out-ranges holds back (doctrine), and its aircraft and land forces fight instead. Examples:
  Chinese frigates with YJ-83 (180 km), and three Turkish frigate classes in the Aegean.
- Naval mode was not changed by the land-air-sea work.
