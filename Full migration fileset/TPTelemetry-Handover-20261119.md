# TP Telemetry: handover, 19 November 2026 (start here)

The working handover: how to work, the files, the pipeline, where each game stands, lessons, and what is open. One thread
works on one game at a time (Alexis, 16 November 2026: `claude/TPTelemetry-Working-in-sections.md` says why and how).
History is not here: `claude/TPTelemetry-History-20261104-20261116.md` has what changed day by day from 4 to 16 November,
`claude/TPTelemetry-History-20261117.md` the 17 November Flycast work, `claude/TPTelemetry-History-20261118.md` the
18 November Supermodel work (Scud Race, Daytona USA 2 Power Edition) and SWDC from its exe (plugin 0.19.0),
`claude/TPTelemetry-History-20261119.md` SWDC checked in her race, Initial D 3 moved to Flycast v2.7-1, plugin 0.20.0's common dashboard values and lamps and 0.21.0's wheel decoders, and the 4 November reference (`claude/TPTelemetry-Reference-20261104.md`, the old handover) the architecture, formula language, design rules,
licensing notes and older history (its version numbers, test counts and file lists are out of date). Open those, and the
other games' notes, only when the work needs them: whatever is read stays in the conversation and costs on every turn.

**Working with Alexis:** Australian English, no em dashes, first person, business-like but approachable, concise.
**Per-game tuning stays per-game** (Alexis, 11 November): when she asks to change a common value's curve for one game,
change only that game's clause in `commonrows.py` and check the pack diff shows no other profile changed.
**Times on screen:** she calls the time left RT and the race clock TT (total time); give both with the recording time.
**Give TT as minutes:seconds.hundredths** (1:02.35, not 62.35 s) (Alexis, 16 November 2026).
**Every bit of telemetry is valuable** (Alexis, 10 November): especially in arcade games, which are less realistic and
more showy, so record and map anything the game shows or does (warnings, boosts, spins, wheelspin, checkpoints, corner
markers), not only the physics.
**Effects only while racing; lamps always** (Alexis, 11 November): effects run from the start to the finish line, so each
game's `car_in_race` must end at the line (many games drive the car after it); lamps light in the attract mode and menus
as on a cabinet, so never tie a lamp row to racing. The pack README's Common values section lists which games' finish is
set and which still need a recording through the finish.
"Just advise" means change nothing. Serve every deliverable each time, in sensibly named archives, each file once
(no loose copies of anything inside a zip; READMEs only inside the zips), always the complete archives (see Files). If something in a request is unclear, or a
file she says she'll attach isn't there, stop and ask before doing the rest; and a question to her should say what to do,
not only ask. When asking about a recording, number the events yourself and give the recording time and the race time
(RT) she can see on screen; don't refer to her marks by number.

## Files (19 November 2026)

Four archives plus this handover, always served complete and together:

* `TPTelemetry.zip`: plugin source **0.21.0** (helper **0.18.0**; 19 November, 15:00: USB capture decoders for Thrustmaster,
  Fanatec, Logitech HID++ and USB PID wheels (Moza and others), `TPTelemetryFfbTest.exe` and Record Test Pattern;
  14:10 (0.20.0): common dashboard values and common lamps, `Common_Properties.md`, `Common_Dashboard.md`, `Common_Lamps.md` help files; 18 November, evening: `[rel]*size` offsets index an array of records by a number the game keeps beside its pointer, for SWDC; 17 November: `@` offsets follow a game's 32-bit pointers inside an emulator's memory, for Flycast), the tests with every game's
  recording fixtures (and since 18 November Scud Race and Daytona 2 PE logs from the emulator, four Scud and three Daytona 2 PE recordings of hers; 19 November her SWDC-11 race), README and help files
  (`Formula_Reference.md` is the formula language).
* `TPTelemetry-GameProfiles.zip`: the pack, **107 profiles** in emulator folders, README (hand-edited), Game_Setup.md and
  Game_Formulas.md (generated over every profile).
* `TPTelemetry-Tools.zip`: every script: the pack generators, `i960.py`, `tptram.lua`, `m2ramscan.py`, `exe/` (Windows exe
  helpers), `elf/` (`elfsyms.py`, for Linux game programs), `sh4/` (Flycast save states and SH-4 code, 17 November) and
  `supermodel/` (18 November: a logger patch for Supermodel's source, headless run and key scripts, a RAM log reader, value
  finders, an autopilot, a fixture writer, PowerPC ROM tools) and `ue4/` (18 November, evening: Unreal Engine 4 exes:
  reflected class properties, native function tables, RIP-relative xrefs) and `swdc/` (19 November: `capfixture.py`). Its README lists each script and its last change.
* `TPTelemetry-ProjectFiles.zip`: every note in the project (`claude/*.md`) except this handover.
* `TPTelemetry-Handover-20261119.md`: this file.

**Archive sizes:** write them with plain `zip -r` (keeps the per-file timestamp and owner fields her copies carry); the 18 November
delivery used `zip -X` and came out a few KB smaller with files added, which looked like a loss (19 November). Compare file lists and
contents, not zip sizes.
**Why complete:** the generator rebuilds every generated profile and both generated docs on each run, and the tests read
the whole pack, so a pack with one game in it would come back with other games missing or rebuilt from scratch. The
archives cost nothing until a file is opened; read only what the game in hand needs.

The claude.ai project holds the same notes (`claude/`) and older copies of the scripts (`claude/tools/`, which lag: take
scripts from the Tools archive). Alexis keeps her recordings, Cheat Engine tables, the sample `.siprofile` files,
`TP-Telemetry-game-values.csv`, and the game programs she has attached (MD5s in each game's exe note: WMMT3DX+ `main`,
OutRun 2 `jennifer`, `GRID.exe`, Sega Rally 3 `Rally.exe`, and the GTI Club, ID6, BG4, Mario Kart exes; `flycast.exe` v2.5-219-g27a913fc0
and (19 November) v2.7-1-g44e4c7b50, MD5 d8db8f69..., and `initdv3e.zip`, BIOS and keys only;
`supermodel.exe` 0.4-dev git 0b82a98, MD5 440c8330..., `scud.zip`, Supermodel's `scudau`; `dayto2pe.zip`, `daytona2.zip` and her `dayto2pe.nv`;
SWDC's `Todoroki-Win64-Shipping.exe` v2.30, MD5 94ed0ba0...).

**Pack generator:** `python3 -I tools/memgames.py <pack copy>` (needs `../ffbsrc/repo` and `../refsrc/ob`: clones of
Boomslangnz/FFBArcadePlugin and Boomslangnz/OutputBlaster beside the tools folder). The pipeline is memgames, moregames,
oblamps, `moregames.fix_m2_lamps`, `moregames.fix_mk118_lamps`, `moregames.fix_mkusa_lamps`, commonrows, tidytable,
gameformulas, gamesetup. A second run changes nothing; run it on the unchanged scripts first and check the pack comes out
identical. The pack README is hand-edited; everything else is generated (hand edits go in the scripts, never the JSON).
A single-use memory read gets merged into the row that uses it (tidytable): to keep a raw read as its own row, don't give
it exactly one user. After a change, diff the pack: only the game in hand (and the generated docs) should differ.
**Tests:** in `tests/CoreTests`, `dotnet restore --source <empty folder>` then `TPT_PROFILE_DIRS=<pack> dotnet run
--no-restore`: **2289 pass with the pack, 847 without** (19 November, 15:00). In the cloud workspace `apt-get update` then
`apt-get install dotnet-sdk-8.0` works (Ubuntu 24.04), pip has pefile and capstone, and the two FFB repos clone from
GitHub. `sh tests/CompileCheck/compile-check.sh` (plugin, helper and test target build) also needs mono-devel (`apt-get install
mono-devel` works; passed 19 November, 15:00, with plugin 0.21.0, helper 0.18.0 and the test pattern tool).
**Exe reading:** copy the exe to `bin.exe` in a folder; `exe/dis.py` writes `ins.pkl`, `rtti.py` `vt.pkl`, `vtables.py`
`vts.pkl`; then `fn.py`, `around.py`, `field.py`, `xref.py`, `sref.py`, `callers.py`, `fstart.py`, `calls.py`, `rawref.py`
(`python3 -I`). **32-bit and 64-bit** since 16 November: `dis.py` writes `base.txt` (image base, bits) for the others,
and in 64-bit code RIP-relative operands are absolute addresses, so `xref.py` works the same (Initial D Zero, IDTA, SWDC).
**Unreal Engine 4 exes** (SWDC; IDTA is UE4 too): `ue4/` works without a full disassembly (SWDC's 49 MB of code): `uprops.py`
names a class's properties and offsets, `natives.py` its native functions, `ripref.py` finds code using a global.
**Linux game programs** (BudgieLoader games): `python3 -I elf/elfsyms.py <program> <regex | 0xaddress>`; `objdump -d -C -M
intel` for the code. Absolute addresses, Module blank.

## Common values, dashboard and lamps (all games; 19 November, plugin 0.20.0)

* Three common sets, each from unticked rows `commonrows.py` adds to every profile, published from whichever game is selected:
  * **Effects** (`car_<name>` -> `TPTelemetry.Car_...`, 0 outside a race): what the "TP -" effect profiles read.
    Reference: the plugin's `Common_Properties.md`.
  * **Dashboard** (`car_speed_kmh`, `car_position`, `car_lap`, `car_time_left`...; plugin adds `Car_speed_mph`,
    `Car_max_speed`, `Car_max_revs`, `Car_rpm_dial`, `Car_gear_text`, `Car_final_lap`): not gated by the plugin, but
    added before each game's own race gating, so games she asked to read 0 outside a race do. `car_speed_kmh` only where
    the units are known (`KMH`/`MPH` in `commonrows.py`). Reference: `Common_Dashboard.md`.
  * **Lamps** (`lamp_<name>` -> `TPTelemetry.Lamp_...`, 23 names, never gated; stand-ins for view, leader, shift, boost,
    brake). One `Lamp_warning` for every on-screen warning (her choice). Reference: `Common_Lamps.md`.
* `lamp_` is reserved for the common lamps: Supermodel's own lamp rows are `lampstart`, `lampview1`... since 19 November.
* A new game: give it `car_speed_kmh` once its speed units are confirmed, and `lamp_` rows for any lamps it has
  (the generic names `lampstart`, `lampview1`, `lampleader`... map on their own). Game_Setup.md lists each game's.
* Docs: the three files above live in the plugin folder (TPTelemetry.zip) and in the claude.ai project under `docs/`
  (not `claude/`, so the ProjectFiles archive doesn't carry a second copy); a test checks they name every property.
  Update both copies together.

## USB capture wheels (19 November, plugin 0.21.0)

* Decoders: Logitech classic (G25 checked; G27, G29, G923 PS...), Logitech HID++ (G920, G923 Xbox, G PRO, RS50),
  Thrustmaster (T300RS family), Fanatec (CSL, DD, ClubSport, Podium), USB PID (Moza, Simucube, VRS, Cammus, Asetek,
  PXN, OpenFFBoard), Xbox pads. All but the G25 written from protocol descriptions (Linux drivers, USB PID spec) and
  **not yet checked**: the first capture from an owner settles each, including which way + is.
* USB PID layout: Windows' hid.dll on the PC (`WindowsHidPid.cs`), else the report descriptor if a capture sees the
  wheel plugged in (USBPcap's injected descriptors don't include it), else the shared layout guessed (status line says
  which).
* **Record Test Pattern** (`TPTelemetryFfbTest.exe`, DirectInput, its own process): known forces at known times, with a
  `.txt` log beside the capture. Ask owners for that capture plus the log, make, model, firmware and mode
  (USB_Capture.md, "Capturing a racing wheel"). The tool hasn't run on a real wheel yet: its first run is a check too.
* No checked game needs USB capture; on a wheel it adds road bumps and cornering to Battle Gear 4 Tuned, and road bumps
  and missing wall-hit sides to OutRun 2 SP. The hook games (CARS, FNF, Cruis'n Blast, Hummers) rely on it most.

## Where each game stands (18 November)

* **BG4JP** (Battle Gear 4 Tuned): checked in nine recordings (`claude/BG4JP-capture-20261110.md`). Speed, revs, gear,
  position, leader, race phase (6 countdown, 7 racing, 8/9 finish, 11 time up), `live` (race clock moving; 0 when paused),
  time left, checkpoint, lap, nitro and boost, wall hits and scrapes by side, car contact on four sides, wheelspin,
  drift angle and drift (heading vs travel), Brake! and the curve markers (car navigation: `brake_warning`,
  `corner_ahead`, `corner_side`, `corner_sharpness`). Max speed 200, Max revs 8000.
* **ID6** (Initial D 6 AA v1.2): checked in five recordings (`claude/ID6-exe-20261108.md`, `claude/ID6-capture-20261109.md`);
  drift angle from the physics, Car_drift with Alexis's weighting: `(1 - (1 - x)^2) x sqrt(s)`, x = (|angle| - 5) / 45,
  s = (km/h - 20) / 80, both clamped 0 to 1 (5 degree dead zone, full at 50, eased; nothing under 20 km/h, full from 100).
  **Start every other Initial D game's drift from this curve** (Alexis, 10 November).
* **MKGPDX118** (Mario Kart GP DX 1.18): checked in two races and the attract demo (`claude/MKGPDX118-capture-20261111.md`):
  race state, speed, revs, steering, drifts (side, angle, strength), hops, boosts, glides, landings, ripple strips, water,
  hits and their side, spins and item flips, coins, the item lamp, surfaces on two courses (her names), place, laps, race
  time; no effects in the attract demo or after the line. Max speed 160, Max revs 10000. Still missing: drift meter stage,
  LED colours.
* **MKGPDXUSA** (Mario Kart GP DX USA): checked in two races (`claude/MKGPDXUSA-capture-20261111.md`), same rows as v1.18
  at its own addresses, plus bomb blasts and metal grilles. Alexis's USA exe is a modified build that uses more RAM (her note).
* **Surface codes in both Mario Karts are the game's surface kinds**, not looks (13 is jungle floor and striped glass): the
  full list with her names is in the `terrain` row note and Game_Setup.md.
* **WMMT3DXP** (WMMT3DX+): the force rows (joints, hits, walls, slides) checked in the 29 October and 1 November recordings.
  Since 11 November (evening) every memory row reads the game's own variables, found in its `main` with their names
  (`claude/WMMT3DXP-exe-20261111.md`): speed, revs, gear, per-car limiter, turbo and full boost, blow-off, backfire, slip
  angle, wheel slip, wall and car contact, odometer and rival gap, race state (in race = 2, ends at the line), time left,
  race clock, sections, wrong way, throttle, brake, steering, tyre load and airborne. **Checked in her 18:36 recording**
  (`claude/WMMT3DXP-capture-20261111.md`) and her answers, plus a traffic recording (20:15): speed, gear, effects from the
  countdown (driver's view) to the line (not the story movie, not paused), time left, race clock, rival gap, slide and
  steering signs, jump and crests (Car_airborne 1 / 0.3), car and traffic hits (`hit_car` into Car_impact). `rpm` = engine
  (Max revs 10000), `tacho` = the dial (needle's travel ends at the 8k mark of a 9k dial). Car hit side from slot 9's
  sign on car hits (59, opposite to walls), checked against her answers.
* **OUTRUN2SP**: checked in four recordings (2 and 3 November). Since 11 November (evening) also rows from the game's own
  variables (`claude/OUTRUN2SP-exe-20261111.md`; `jennifer` is not stripped): speedo km/h, drift state and rear slip angle
  (Car_drift), slipstream, SHIFT UP, crash motions, main sequence (Car_in_race = `racing`: GAME, no goal or time up),
  stage, race and stage clocks. **Checked in her 19:06 recording** (`claude/OUTRUN2SP-capture-20261111.md`): GO to the
  finish line, the speedo (now Car_speed, km/h, Max speed 280), stage gates, crash motions (kind 7 rear-end with the rear
  lifting, kind 1 flip: Car_airborne), drift sign (+ right), slipstream, the fork's easy arrow (`fork_sign`, 3 s), OutRun
  mode = game_mode 1, the dash's redline light (`shift_light`, 8,380 RPM). Car_drift full at 75 degrees for this game only.
  12 November (`claude/OUTRUN2SP-capture-20261112.md`): car hits and their side from the game (`car_hit`, `car_side`,
  no USB needed), `game_mode` 0 Time Attack / 1 OutRun / 2 Heart Attack, time up, attract mode (Car_speed/rpm/gear/G 0
  outside a race, this game only), redline light 8,270 on four cars, Heart Attack hearts and `challenge_won`. Open:
  Car_drift by feel, SHIFT UP (never shown), observer bit 64.
* **GRID** (parked by Alexis, 16 November): speed, revs, per-car limiter, gear, throttle, brake, steering, shift lights,
  hits and wall side checked in recordings of 3, 4 and 12 November (`claude/GRID-capture-20261103.md`,
  `claude/GRID-capture-20261112.md`). From `GRID.exe` (`claude/GRID-exe-20261112.md`, `-20261116.md`): the race clock
  object, so `racing` → Car_in_race (GO to the line, off paused and at TIME OVER), `race_clock` = the TT on screen, laps
  and lap lines; every Car_ value 0 outside a race. **Open, flagged for when she comes back to it:** (1) time left and
  checkpoints: only in the `StateRaceCountdown` state (+3C float seconds, +E8 checkpoints), no fixed chain: a Cheat
  Engine pointer scan (plan at the end of the 16 November exe note); (2) `position`/`leader` and `game_mode` (7
  Championship, 8 Quick Race) from the exe, not yet read in a recording; (3) the side of car hits (`car_contact` counts
  every car's contacts, not only hers; `wall_side` gave five of eight car-hit sides).
* **SR3** (Sega Rally 3): speed (mph), revs, gear, force, hits and side, airborne checked on 2 and 12 November; the rows
  from `Rally.exe` (`claude/SR3-exe-20261112.md`) checked in her Quick Race of 16 November (`claude/SR3-capture-20261116.md`):
  player state (0 menus, 1 intro, 3 racing, 5 from the line) → `racing` → Car_in_race; checkpoints and lap lines; lap;
  position (held 0.1 s); WRONG WAY (gated by racing); `race_time` (TT) and `rt` (RT as the screen shows it: time given
  less TT in Quick Race, game mode 5; the screen shows an extension about 2 s after the game adds it). Every Car_ value 0
  outside a race. `hit` tuned from her answers (a slide and post-landing jitter no longer count). Front hits read
  whichever side the game gives (Alexis: fine). Open: mode 6 (time_left counts itself down; not yet seen), surfaces and
  water (the physics `CVehicle` path), the motion-seat driver.
* **ID0V230** (Initial D Arcade Stage Zero v2.30, `InitialD0_DX11_Nu.exe`, 64-bit, MD5 2e506fe5...): rows from the game's
  own code (`claude/ID0V230-exe-20261116.md`; `moregames.id0_rows`), the ID6 engine (cGameSession `<main>+18B4E50` → cRace
  +290 → cars +38/+40 → cMoriCar), both slots and yours. **Checked in two recordings, 16 November**
  (`claude/ID0V230-capture-20261116.md`): the tutorial intro race (Trueno, manual) and an Irohazaka time attack (RX-7,
  automatic) and a story race (Silvia, automatic, rev counter readings): speed (the speedo), revs (the counter: manual
  P+B08, automatic P+B04 x 0.885), gear,
  steering (game + left, turned round), drift angle sign, race clock, time left, checkpoints, places (`gap`, rival races
  only), contacts and wall sides, gutter, leaves, landings, ripple strips (vibration level 5), in_race off in the tutorial's
  how-to demos. Max speed 220, Max revs 8140. She runs it under segatools v5; TeknoParrot the same (same exe, game memory
  only). FFB feels kicky and weak (the game's own force); effects don't depend on it: Car_cornering comes from steering and
  speed. Lamps made from race values (`lampleader`, `lampcheckpoint`; no OutputBlaster entry). Open: grass, drains, oil.
* **FLYINITD3** (Initial D Arcade Stage Ver. 3 in Flycast; **wrapped up 17 November**): notes
  `claude/FLYINITD3-flycast-20261117.md` (Flycast's RAM), `-states-` (save states), `-capture-` (eleven recordings, her
  answers, every row's evidence). **Since 19 November for Flycast v2.7-1-g44e4c7b50** (64-bit, MD5 d8db8f69...): emulated
  RAM is `<main>+29D0C10` -> + (game address & 0x1FFFFFF) (`moregames.fly_ram`, `FLY_RAM`; v2.5-219-g27a913fc0, where all
  the checking was done, was `<main>+2840C90`; how to find it in a new build: end of the `-flycast-` note); the game's own pointers are followed with `@` offsets (plugin
  0.18.2, helper 0.17.1; `moregames.fly_ptr`). Process `flycast.exe`; Max speed 205, Max revs 9000. **All checked in
  recordings** (Civic, GT-R, RX-7, Trueno and an 8000-rev car; Akina Lake, Irohazaka, Akagi, Myogi; story and time trial;
  manual and automatic; through the line and time up; `Fly3Tests.cs`, two fixtures):
  * Player's car (static, 0x0C9008A4 to 0x0C9017F8): `speed` C2C (m/s x 3.6), `rpm` EA4, `gear` E88 (0 on the grid),
    `gap` 1650 (ADVANTAGE), `thr_revs` E9C and `rpm_top` EA8 -> `rev_limit`, `rpm_frac`, `throttle`; `steer` BB8 (turned
    round, + right); `contact` B40 with `kick_x` C48 (side, - left) and `kick_y` C4C -> `wall_contact`, `hit`; surfaces
    A74 to A80 -> `rumble_strip` (code 20), `gutter` (18, 32, 48, 49), `grooved_road` (21); `yaw` 8C0 against the travel
    `move_x`/`move_z` 8B0/8B8 -> `drift_angle` (+ right-hand drift) and `drift` (ID6's curve full at 45 degrees: her
    hardest drifts 0.62, as she asked; not raised during a contact).
  * Race object [0x0C8FF174]: `rt` +418 -> +8, `race_clock` +588, `lap` +590 (1/6000 s); `race_ok` (it holds rubbish for
    0.6 s as a race is cleared); `racing` = live, gear > 0, race_ok, clock moving -> Car_in_race (GO to the line or time
    up); `lampleader` (from GO, gap >= 0, held until the race is cleared, as ID0) and `lampcheckpoint` (time left jumping
    up, 2 s): made from memory.
  * Common: Car_in_race, speed, rpm, gear, accel_g, throttle, rpm_frac, impact(+side), wall_contact, rumble_strip,
    off_road (gutter), drift, drift_angle, grip_loss, cornering (from steering and speed, as ID0), all 0 outside a race;
    Car_road_bump and Car_vibration from Flycast's `midiffb` output (read 0 with outputs off).
  * Outputs are optional: with the FFB Arcade Plugin's files in Flycast's folder the game froze in the menus (removed).
    Lamp bit 3 blinks through the race; nothing follows the lead. `ram_probe` (removed 17 November, back 18 November
    at her request): a steady 3509307139 means the RAM pointer is right for that Flycast build.
  * **Open if she returns to it:** hits from the rival from behind show in nothing; other courses' surface codes; how
    Car_drift and Car_cornering feel on the rig with shakers; her FFB result (Flycast's haptic power, or the FFB plugin
    alone). The rom zip has no program: code is read from a save state's RAM (`sh4/flystate.py`, `sh4/sh4dis.py`).
    Other Flycast profiles untouched (outputs only).
* **SMSCUD** (Scud Race in Supermodel; started 18 November, **checked in four recordings** the same day): notes
  `claude/SMSCUD-supermodel-20261118.md` (the RAM pointer, the emulator method, every address) and
  `claude/SMSCUD-capture-20261118.md` (her recordings, changes, questions). Supermodel 0.4-dev git 0b82a98 (64-bit, MD5
  440c8330...): RAM is `[[<main>+AE6280] + 8] + game address` (`moregames.sm_ram`; bytes at A ^ 3, 16-bit at A ^ 2).
  Process `supermodel.exe`; Max speed 350, Max revs 9500. Race sequence 0x10525C (phase, Beginner's rolling-start
  countdown, top byte = finishing place less 1) -> `racing` (GO or the 3-2-1 to the line or time up), `final_position`,
  `finished`; time left 0x104010 and race clock 0x10400C (frames, 57 a second); `pit_stop`; car record 0x181000 (0x300
  each, 40 cars): car +105 (4 Porsche, 5 Viper, 6 F40, 7 McLaren) -> `rev_limit`, speed +118, revs +244, gear +23C, pedals
  +1E7/+1BF, steering +1C0, laps +198, position +1D4, lap times +1CC/+1D0/+1D2, crash timer +1B7 (big crashes only), road
  strips +288 to +298 (`wall_contact`), car contact +2F7 and near car +101 bit 4 (from the emulator), movement +178 to
  +180 (`airborne` from free fall, `light` over crests) and heading -> `drift_angle` (Car_drift straight 14 to 58
  degrees, from her F40 points). Car contact sides and wall sides confirmed in her F40 recording; REVERSE! matches
  `wrong_way`. Car_in_race also on the grid on Medium and Expert (revs and gear, and Car_throttle 1 so the engine follows
  the revs: her answer). F40 limiter 9040 (her fifth recording). **Open:** the grass verge reads as the far wall (the
  road strips come in lanes: capture note).
  NVRAM: LINK ID SINGLE.
* **SMDAYTO2PE** (Daytona USA 2 Power Edition in Supermodel; new 18 November, 15:00; she works on the Power Edition
  only, `SMDAYTONA2` stays force and lamps): values found in the emulator from her ROMs and NVRAM
  (`claude/SMDAYTO2PE-supermodel-20261118.md`): in-game byte 0x1065E7, time left 0x105010, race clock 0x10500C, car
  record 0x501000 (0x1000 each). **Checked in three recordings** (17:10, `claude/SMDAYTO2PE-capture-20261118.md`):
  `racing` GO to the line or time up; Car_in_race from the start (rolling start kept, her wish; the grid on Advanced and
  Expert, with Car_throttle 1 there); car class 1 NORMAL, 2 HARD, 3 SPECIAL; car contact +0x3C0, its side from the
  game's roll command (`force`: roll right for left hits; front-corner hits send 175, no side); `pit_stop` (time left
  still; effects stay on); Max revs 8500 (8.2k on the dial), and `rev_limit` by class from two limiter recordings
  (EASY and SPECIAL 7650, NORMAL and HARD 8500) for Car_rpm_frac; Car_drift fitted to her three points (her answers
  17:35). One player record whatever the car. **`rpm` is the rev counter, +0x1E0** (19:10, from her SPECIAL standing start:
  it shows wheelspin and clutch slip; +0x1DC, the wheels' revs, is `rpm_wheels`). **Open:** front-corner car-hit side; whether speed should weigh
  more in Car_drift (all her drifts were at 260 to 295 km/h); the other courses.
* **SWDC** (Sega World Drivers Championship v2.30; from its exe, 18 November; **checked in her GT300 and GT500 races of
  19 November, SWDC-11 and SWDC-12; wrapped up 19 November, 12:30**): `claude/SWDC-exe-20261118.md`, `claude/SWDC-capture-20261119.md`. UE 4.17; the player's physics are blueprint
  variables (the cheat table's speed2/gear2 go through UE's name-hash map). The exe's `UCarLibrary` keeps native per-car
  records: data model Q = `[[[<main>+4B07438] + 278] + 10]`, your car number Q+16D0 (from 1; 0 in the menus), records
  `[Q+2550]` 0x578 each, lap time limit Q+2C18 (300, then 180 at each lap line). Your record (plugin 0.19.0's
  `278, 10, 2550, [-E80]*578, F - 578`): race state +2E0 (1 loading and countdown, 2 from the rolling start, 3 from the
  line, 4 to 6 retired) -> **`in_race` = state 2; every game value and every Car_ value but Car_in_race is 0 outside it**
  (Alexis: from the rolling start to the line or time up). km/h +460 (the speedo: Car_speed, Max speed 230), gear +490
  (= gear2), brake +480, place +410, class place +420, lap +1E8, sector +1D8, clock +4B8 (TT), position +2F8, yaw +314
  (drift angle against travel, + right-hand: checked). Car_rpm = the table's `revs` (the record's +470 isn't the engine).
  `position` = class place +420 (what the screen and results show), `overall` +410. **From her answers (11:00):** car
  contacts and walls from the sideways shove (`jolt` from position and heading, `car_contact` over 4 G while not
  sliding: all twelve found, sides right) -> Car_impact/side; head-on hits from the speed (`hit`, 5 G while braking);
  verge and solid kerbs from the game's vibration command (strength and period) -> Car_off_road, Car_rumble_strip;
  Car_drift = 0.072 x (|angle| - 10)^0.6 x sqrt(min(km/h, 150) / 100), fitted to her seven slides; the contact side is
  the first shove's (12/12 GT300, 8/10 GT500). Sparks come on downshifts under
  braking (unused). `checkpoint` (sector and lap lines, three a lap) ticked: no time extensions in this game. Her GT500
  race (SWDC-12, 11:30): class place = overall at the front. Attract mode reads 0. **Open** (end of
  `claude/SWDC-capture-20261119.md`): solid kerbs on the GT500 course send no vibration (Car_rumble_strip 0 there); a car
  pushing from the inside of a corner read the wrong side; Car_drift over 0.1 about 11% of a race.
* **GTICLUB3**, **M2STCC**, **M2DAYTONA**, **M2SRALLY**, **WMMT5DXP**:
  checked in earlier recordings (notes per game).

## Lessons learned (4 to 19 November)

* **USBPcap's injected descriptors stop at the configuration descriptor** (19 November, from its source): a HID device's
  report descriptor is only in a capture when the device is plugged in during it. Windows' hid.dll knows the layout
  (HidP_* functions), which is the way to read USB PID wheels on the PC.

* **A game that shoves the car at contacts gives their side:** SWDC moves the car sideways 4 to 10 G at every car contact and
  wall hit (from position and heading on the game clock), always away from the side hit; gate out slides, whose own swing
  does the same. Its vibration command's strength and period told verge, solid kerb and braking judder apart.
* **A getter's name doesn't prove the blueprints feed it** (SWDC, 19 November): `GetRevsPerMinute`'s field held 0, 8000 and odd
  values while the engine ran 7,000 to 12,000; `GetKilosPerHour` and `GetShiftPosition` were exact. Check each record field
  against a known value in her first recording.

* **Unreal Engine 4 games name themselves through reflection** (SWDC, 18 November): each class's `Z_Construct_UClass_*`
  builds its properties with the name and offset side by side, and the native functions sit in (name, exec) tables. Read
  those before disassembling 49 MB of code. Blueprint-only classes aren't in the exe: a cheat-table chain into one can go
  through the engine's name-hash map (SWDC's speed2), so look for the C++ library the blueprints write their values to.
* **An array indexed by the game's own number needs an indexed offset** (plugin 0.19.0, `[rel]*size`): check whether the
  record has a field that should equal the number (SWDC's car number at +8) and gate on it (`player_ok`).

* **An open-source emulator can be run here to find a game's values without recordings** (Scud Race, 18 November):
  build the exact source of her exe (its version string names the commit), add a per-frame RAM logger, run it headless
  (Xvfb, Mesa software OpenGL, `xdotool` for keys) and read the HUD off screenshots. An autopilot that steers along the
  computer cars' recorded paths gives clean laps; poke extra time to reach the finish. Ask her for the ROM set, not save
  states, when the emulator is open source.
* **Big-endian guests in a little-endian emulator:** Supermodel keeps PowerPC RAM as host-order 32-bit words, so bytes
  sit at A ^ 3 and halfwords at A ^ 2 (Flycast's SH-4 is little-endian and needs none of this).
* **Arcade clocks can count at a rounded rate:** Scud Race's clocks are frames, divided by 57 on screen though the
  machine runs at 57.524 Hz; check a result screen's time against the frame count before choosing the divisor.
* **Free fall is a clean airborne test:** in the air a game's vertical movement per frame changes by exactly its gravity
  each frame (Scud: 0.00287 m), which a slope over 0.15 s picks out; a game's own "air" flags may be timers.
* **A value that follows the keys can still be something else:** Scud's +2C4 tracked key steering in the emulator but not
  her wheel. Check every input-like row against USB in her first recording.
* **Per-wheel "which piece of road" numbers make a wall-contact side** when nothing flags contact: a wheel against a
  wall is on another strip of the course than the car's middle.
* **A contact can swing a heading-against-travel drift angle** (Initial D 3: 32 degrees against a rail where her real
  drifts peak at 21): cap the drift during a contact at its value just before.
* **Ask the target value of an effect, not only its shape:** Alexis gave Car_drift's top (0.6 to 0.65, to match the other
  Initial D games); fitting the curve's full angle to her hardest drifts took one recording.
* **An object being freed can pass a sanity check for a sample** (Initial D 3's race object read lap 8, clock 0.05 once
  while being cleared): gate on several fields at once, on the size of the clock's step, and test on a recording that
  runs back into the menus.
* **Check which way a steering value turns the car before trusting a description of one moment:** the heading's turn in a
  known left hairpin fixed Initial D 3's sign when her memory of one corner pointed the other way.
* **Values found in save states of one race can sit in memory that moves** (Initial D 3's clocks read rubbish in the
  next two races). An emulated game's pointers are guest addresses (32-bit), so a host pointer chain can't follow them.
* **Don't print whole event tables into the conversation:** summarise in the script and print only what a question needs.
* **A Flycast save state carries a screenshot** (a PNG after its header), so the HUD at the moment of saving needn't be
  written down; and values found in two states of one race can still be heap addresses (the clocks): check where the
  code refers to an address before calling it fixed.
* **A GD-ROM game's rom zip has no game in it** (Initial D 3: BIOS, EEPROMs and the security PIC; the program is on the
  CHD, encrypted). Ask for an emulator save state made in a race instead: it is far smaller and holds the decrypted
  program and the live values together.
* **An emulator update moves its RAM pointer but not the game:** Flycast v2.5 to v2.7 changed only the base offset (one
  global); `ram_probe` showed it at once (empty while the outputs still arrived), and the same instruction pattern
  found the new one in minutes. Keep a plumbing row like it in every emulator profile.
* **An emulator's RAM pointer can be found from the memory map's constants** (Flycast: the one place that adds 0x0C000000,
  0x04000000 and 0x20000000 to the same base), even in a stripped exe with its log strings removed.

* **Replay tests catch edge pulses:** SR3's `checkpoint` fired at GO (the time set from 0) and at the start line (the
  line count -1 to 0); a new fixture with its own checks found both. Give each derived pulse a check on a real recording.
* **A game's own vibration levels can name surfaces:** Initial D Zero shakes at level 5 for ripple strips, 4 for wall
  scrapes, 2 for car contact; a surface sound shared by painted and ripple strips couldn't tell them apart.
* **Check a "limit" field against two cars before using it:** ID0's P+B18 read 10000 for both (the gauge's top).
* **A 64-bit exe of a known engine maps across by class name:** Initial D Zero's RTTI named ID6's classes, and the
  vtable constructors gave every structure; constants (3.6, 0.175) and sound-name strings found the same functions.
  Uninitialised .data (the file's tail) reads 0 from the exe: read it in a recording instead.
* **A state machine's states have no fixed chain** (GRID's StateRaceCountdown sits in a tree keyed by state id): when the
  only home of a value is such an object, ask for a Cheat Engine pointer scan, with the instruction addresses that write
  it so she can confirm the value first.

* **Check a recording's columns before analysing it.** Import never overwrites a game, so a new pack's rows only arrive
  after Delete Game and Import; all three 12 November Sega Rally 3 recordings were made with the old SR3 profile.
* **USB steering settles a steering row's sign** without asking (GRID's `steer_in` against `FFB_steer`, -1 left).
* **"Lap time" fields can be race times:** GRID's car record +30 is the car's own race clock, held at its finish, which
  makes a clean "this car has finished" signal.

* **An exe with RTTI only for its libraries can still name its own classes through log strings** (Sega Rally 3's
  `CSR_PlayerState::Execute - changed to PLAYER_STATE_RACING`): find the code that prints them and the state field is
  beside it. Static arrays of per-car records (SR3's 0x9DBAC8, 0x1F18 each) show up as `imul reg, 0x1F18; add reg, base`.

* **Several processes can share the game's name** (an old game.exe hung on after closing; game.exe is a common name):
  two BG4 recordings read the wrong one (all zeros; frozen). Plugin 0.18.1 opens the newest. If every row reads 0 or
  never changes while the game runs, suspect this first.
* **Pauses and attract modes:** a paused game holds every value (BG4: 26 s and 14 s), and a race flag can stay on in the
  menus after the attract demo is interrupted (BG4 in_race), or the demo can run the real race code (Mario Kart). Gate
  racing on something that moves (the race clock) and on the camera or state, not only the game's own flag.
* **A plausible name can be wrong:** BG4's wall bit 2 looked like rough ground in one recording (she described rougher
  road) but matched the game's nitro timer exactly in another. Two independent sources settle it.
* **HUD warnings may come from course data, not the car:** Brake! wasn't the car's brake-assist flag the HUD code
  pointed to; her description of where it appears (under Extended Time!, above the curve markers) led to the car
  navigation task. Ask her what a warning looks like and where.
* **Ask which course and cup with every recording.** Mario Kart's surface codes are kinds shared by different-looking
  surfaces, and she once named the wrong course; the code timeline per lap shows quickly whether two recordings are the
  same course.
* **A latch taken on the trigger sample can already hold the change:** Mario Kart's hit side compared the slide with
  its value as the shake started, but the knock lands on the same samples; take the baseline from before (an `avg` over
  the window before the trigger), and leave out cases where the baseline itself is moving (drifts unwinding).
* **Check what an FFB plugin "force" really is:** Mario Kart USA's is the sine of the slide angle, so it doubles as a
  drift angle. Versions of one game keep the same structures at moved addresses; find a shared function (the engine-sound
  feed) and the rest maps across quickly.
* **One-frame triggers** (the navigation's sign flags) need holding with `maxw`, and fixtures for them keep every sample.
* **A drift angle where the game has no slip value:** the car's heading vector against the slope of its position
  (BG4: position +100/+108, heading +D0/+D8). Check the vector is a unit vector that lines up with travel when driving
  straight before trusting it.

* **BudgieLoader games are Linux programs and may keep their names.** WMMT3DX+'s `main` had 61,106 symbols; read the
  game's own variables at their fixed addresses (Module blank) before trusting cheat-table chains into BudgieLoader.exe,
  which can be copies (WMMT3's was the HUD meter's) and move with TeknoParrot updates. Worth trying on OutRun 2, Initial D
  4 and 5, R-Tuned, Sega Race TV and Cruis'n Blast (`elf/elfsyms.py`).
* **Some BudgieLoader games aren't stripped at all** (OutRun 2's `jennifer`): full names, so cheat-table addresses can be
  named in seconds, and they often turn out to be HUD flags or other cars' fields rather than what their names suggest.
* **A freed object's pointer may stay set** (WMMT3's `CSeq::End`): gate reads on a flag the game clears, not the pointer.
* **MasterRaver's "GTimer1" was the time left in WMMT3DX+** (`yt::CState::fTDec_Rest`), not a race clock. WMMT6RRP's
  `race_timer` comes from the same author's "GTimer1": check whether it counts down too before using it as a clock.

* **A game's own race state can flick on outside a race** (WMMT3's story movie sets state 1 for 0.2 s with the countdown
  held at 4): gate on the state and on a clock that moves.
* **TeknoParrot's slot values can mean different things by event**: WMMT3's slot 9 gives walls 63 (+ right) and car hits
  59 with the opposite sign. A threshold set on walls missed every car hit live; the game's own contact flag caught them.
* **A dial can be scaled away from the engine**: WMMT3's needle tops out at the 8k mark of a 9k dial, with the engine at
  10k. Ask for dial readings at given moments before choosing which revs to use.
* **USB capture is ground truth for hit sides**: with it on, the FFB plugin's push gives each hit's side, so raw game
  values recorded alongside (OutRun 2's contact words and side code) can be matched in one recording. Read new raw rows
  through `valid()` in formulas, or older fixtures without them read NaN as true.
* **Fixtures in the game's units** let new rows be tested on recordings made with older formulas: convert each recorded
  row back through its formula (see `Wmmt3GameTests`, `Or2GameTests`).

* **A "static" Cheat Engine base can be heap.** GRID's `B1B100` and friends are in memory no code refers to (the game's
  own arenas, laid out the same each run), so the exe can't confirm them; read the code from the object outwards
  instead (the dial's own update named every field). Codemasters' commentary facts (name strings with getters) are a
  quick map of where a game keeps race values.

* **Look at the recorded outputs, not just a replay.** The stuck windows only showed in what the plugin recorded live
  (hit 1 with no events, accel_g flat); replaying the raw reads looked fine. Compare the two when something reads odd.
* **Names from sound tables are guesses.** ID6's "drain" bits were the gutter; a recording settles it.

* **Check a port's other uses before patching it.** The old `TPT_LAMP_PATCH` moved the byte instructions at
  0x01C0001E, which in STCC and Sega Rally were the controls read (write 0, then read 8 bytes), not lamps. Sega
  Rally's controls froze; the real lamp write was a 16-bit store the patch never touched. Disassemble the code around
  any port access first (`claude/tools/i960.py <program.bin> <from>-<to>` decodes i960 code).
* **The program ROM:** 16-bit interleave of the two maincpu ROMs (offsets 0 and 2). Absolute addresses show up as
  MEMB instructions with mode 0xC and the address in the next word; a scan for those finds every fixed variable.
* **The RAM logger's samples are end-of-frame; TP Telemetry reads mid-frame.** A timer that read cleanly in the log
  (Sega Rally's checkpoint countdown) was caught at a stray value by the plugin and stuck on. Check new rows in a TP
  Telemetry recording before trusting them, and prefer values that don't change mid-frame (the time left).
* **A value that "goes to 0" may only hold.** Sega Rally's hit strength keeps its last value; detect the change.
* **Game clocks:** Daytona counts 64 frames to the second, Sega Rally 60, STCC uses float seconds.
* **Her marks land just after the event**, and she can read values off video to the frame. When a value can't be
  pinned, give her a timestamp from her recording and ask what's on screen (the tunnel surface was found that way).
* **Old claims get disproved; say so plainly** (the STCC lamp-instruction claim was wrong).
* **Ask for her working files first.** Her STCC leader lamp came from a special OutputBlaster build; disassembling it
  (`push 0x174cf0` in its loop) found the lamp source in minutes, after a day of searching game RAM. Emulator-side
  latches (exe+174CF0, exe+174CF4, exe+17285B) are often simpler than the game's own variables.

## Waiting on Alexis / next likely asks

* Wheel captures (0.21.0, 19 November): a test pattern capture and log from any Thrustmaster, Fanatec, Moza or newer
  Logitech owner, to check each decoder and its sign; her own G25 can also try Record Test Pattern once, to check the
  tool against the decoder that is known to be right.

* Common dashboard and lamps (plugin 0.20.0, 19 November): her first dashboard or LED set-up on them. Likely asks: units
  for Crazy Taxi, D1GP, Storm Racer G and the Flycast driving simulator (a run to top speed with the speedo noted);
  Mario Kart USA's steering sign; which Battle Gear lamp is Hazard; Initial D 3's `lamp0` to `lamp7`.

* SWDC: wrapped up 19 November (no more recordings planned). If she comes back to it: how contacts, drift and the verge
  feel on the rig; kerbs on other courses (the GT500 course's send no vibration). Ask by TT (m:ss.hh, `race_secs`).
* Daytona USA 2 Power Edition (SMDAYTO2PE): the rev counter is found (19:10). Her answers are in; she is moving to a
  new chat. Next: how the
  profile feels on the rig (Car_drift, hits); a slower drift if speed seems to matter. Questions by recording, lap and lap
  time (she prefers "lap 3, 0:41.25"), else TT.
* Scud Race: the verge (later).

* Initial D 3 (Flycast): moved to Flycast v2.7-1 on 19 November (`FLY_RAM` 29D0C10, from her new exe; her 12:39 recording
  with the old pointer read nothing). Waiting: a recording with the new profile, to see `ram_probe` steady at 3509307139.
  For any later build: the pointer to mem_b.data next to the set-up code (end of `claude/FLYINITD3-flycast-20261117.md`),
  then `FLY_RAM` in `moregames.py`; the other rows follow. Other open items under Where each game stands.
* GTI Club: a second recording to check `time_left` and `checkpoint` (the race display copy). 198.84 s stays a
  wall (she wasn't sure: LHS wall or LHS kerb ride; no wheel read kerb, 11 km/h lost).
* ID6: her answer on 15:43 mark 7; how the speed-weighted drift feels; then time left, laps, position and the gap; the 26.87 s car hit in the
  first recording (nothing in any value). Still unchecked: other cars' limiters, time attack and VS (slot 1), landings, joints and
  rumble strips (none on the prologue course).
* BG4JP: how long Brake! stays on screen (held 1.5 s); the curve codes not yet seen (a slight right, other snakes);
  wall bit 4; car_flags bits; other cars' limiters (Max revs 8000 suits the Citroen; the Golf's is 7500, tuned cars 9000).
* Mario Karts: drift meter stage and LED colour digits still need finding in the exe (or a recording with the meter's
  stages and lamp colours noted). New courses: ask her to name any new surface codes (the `terrain` note lists the known
  ones); `off_road` is set per code.
* Finish line: a recording through the finish to the results for Model 2 Daytona, to end Car_in_race at the line
  (WMMT3DX+'s and OutRun 2's were confirmed on 11 November; Sega Rally 3's is now from its exe, to confirm).
* Initial D Zero v2.30: checked in three recordings (16 November); thread wrapped up. If she comes back to it: grass,
  drains or oil if a course has them; how Car_cornering and the made-up lamps feel.
* Sega Rally 3: nothing asked and unanswered. A race in game mode 6 (if it exists in her setup) would check `rt` there.
* GRID: parked (see Where each game stands for the three open items).
* OutRun 2: nothing asked and unanswered. Open (end of `claude/OUTRUN2SP-capture-20261112.md`): Car_drift targets by
  feel; SHIFT UP; observer 64; per-car redline light only if 8,270 feels off on a car.
* WMMT3DX+: a recording with the new profile when convenient (countdown gate, crests, car hits live); the crest level
  and Car_drift by feel. Still to see: checkpoints in a mode with extra time; zebra strips.
* Older open items from the 4 November reference still stand (GRID in-race value, per-car limiters, WMMT6RR Outputs
  script from MasterRaver, SWDC speed1/gear1, licence before release).
