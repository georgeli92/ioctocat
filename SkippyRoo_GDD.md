# Skippy Roo — Lightweight Game Design Doc

## Game Pillars
- **Pick-up-and-play**: One-touch hop with forgiving early-game gravity; fast restarts.
- **Australian charm**: Biomes, wildlife hazards, and audio cues (magpies, didgeridoo drones) sell place and personality.
- **Retro vibe**: 8–16-bit pixel art, chunky UI, CRT vignette, chiptune-didge soundtrack.
- **Variety through events**: Rotating biomes and weather bursts change jump timing and silhouette clarity.

## Core Loop
1. Tap/hold to hop and maintain altitude while the world auto-scrolls.
2. Collect **Gumdrops** to build the **Boomerang Combo** multiplier.
3. Dodge obstacles; survive weather events; grab power-ups to extend runs.
4. Hit an obstacle → lose shield (if active) or end run → show score, missions, cosmetics, quick restart.

## Controls & Feel
- **Input**: Tap = short hop; press 0.15–0.3s = medium; press 0.3–0.45s = tall arc. Double-tap within 300ms = **Roo Rush** (short forward dash).
- **Gravity**: Mild; tuned so neutral tap cadence is ~140 BPM in Outback pace band.
- **Camera**: Slight lookahead; 2.5 tiles ahead. Screen shake toggle; heat-shimmer overlay in Outback.

## Scoring
- **Gumdrops**: +1 per pickup; chain without gap to grow **Boomerang Combo**: x1 (base), x2 (5+ chain), x3 (15+), x4 (30+). Chain resets on obstacle hit or 2.5s without pickup.
- **Distance**: +1 per meter equivalent.
- **Style**: Passing through narrow gate (+5) or near-miss magpie swoop (+3).

## Obstacles
- **Static**: Spinifex mounds (ground), termite towers (mid), billabong pools (require hop timing), boulders (tall).
- **Moving**: Swooping magpies (arc), bouncing wallabies (parabolic), dust devils (drift between lanes), tram wires (city, horizontal), low seagulls (reef, straight).
- **Breakable**: Dry branches and signposts (Roo Rush breaks them; otherwise hit).

## Power-ups
- **Boomerang Shield**: Spins around Skippy; blocks one hit; lasts 12s.
- **Didgeridoo Drift**: Lowers gravity by ~25% for 8s; floaty hops.
- **Bilby Burrow**: Downward dash; clears aerial threats; small landing stun (0.2s reduced control).
- **Wattle Honey** (unlock later): Slow-mo for 2s when danger near (cooldown 12s).

## Biomes & Events
- **Outback**: Uluru silhouettes, heat shimmer; hazards: spinifex, termite towers, magpies.
- **Eucalyptus Forest**: Tree trunks as pillars, fog; hazards: wallabies, falling branches.
- **Reef Coast**: Tidal foreground, seagulls, low clouds; hazards: rising tide arcs.
- **City Twilight**: Parallax skyline with Opera House; hazards: tram wires, neon signs.
- **Events** (every ~500m, 8–12s): Wind gusts (push up/down), light rain (reduced friction), heat haze (slight blur), dusk shift (reduced contrast; UI outline).

## Progression
- **Milestones** (distance in a single run):
  - 400m: Unlock palette swap (Sunset)
  - 800m: Unlock **Bilby Burrow** power-up
  - 1200m: Unlock City Twilight biome
  - 1800m: Unlock Wattle Honey slow-mo
- **Meta**: Daily/weekly missions (e.g., “Survive 3 events”, “Collect 50 Gumdrops”), cosmetic shop (hats, boomerang skins), optional rewarded ad for single revive.

## Accessibility & Settings
- Colorblind palettes, screen-shake toggle, reduced weather VFX, adjustable tap sensitivity, left-handed UI option.

## Tuning Tables
### Pace Bands by Distance (per run)
| Distance (m) | Scroll Speed (units/s) | Gravity | Obstacle Density (per 10s) | Event Chance |
| --- | --- | --- | --- | --- |
| 0–300 | 3.2 | 1.0x | 2–3 | 0% |
| 300–700 | 3.6 | 1.05x | 3–4 | 15% |
| 700–1200 | 4.1 | 1.1x | 4–5 | 30% |
| 1200–1800 | 4.5 | 1.15x | 5–6 | 45% |
| 1800+ | 5.0 | 1.2x | 6–7 | 60% |

### Spawn Mix per Biome (weight per 100 spawns)
| Biome | Static | Moving | Breakable | Power-up | Notes |
| --- | --- | --- | --- | --- | --- |
| Outback | 45 | 30 | 10 | 15 | Magpie swoops scale with distance bands |
| Eucalyptus | 40 | 35 | 10 | 15 | Wallabies add vertical mix; fog lowers contrast |
| Reef Coast | 35 | 40 | 10 | 15 | Rising tide counts as moving hazard |
| City Twilight | 35 | 35 | 15 | 15 | Tram wires occupy upper lanes |

### Power-up Drop Logic
- Base drop roll every 7–9s while player is alive.
- If player has active power-up, halve drop chance.
- If player hit in last 5s, bias toward **Boomerang Shield**.
- No duplicate drop for the same power-up currently active.

### Weather Event Effects
| Event | Effect | Duration | Notes |
| --- | --- | --- | --- |
| Wind Gust | Vertical force ±10% gravity; nudges up/down lanes | 8–10s | UI arrow shows direction |
| Light Rain | Reduces friction → slightly longer hops | 8s | Add puddle SFX |
| Heat Haze | Edge blur/vignette; silhouettes harder to read | 10s | Lower contrast; outline player |
| Dusk Shift | Darkens palette; UI gains bright outline | 12s | Slightly reduces spawn density |

## Content Roadmap (First 2 Sprints)
### Sprint 1 (Week 1)
- Implement Outback biome tileset (foreground/background), CRT UI frame.
- Core movement + collision; Roo Rush; Boomerang Shield + Didgeridoo Drift.
- Gumdrop collection, combo multiplier, score UI, run reset flow.
- Pace bands for 0–1200m; obstacle spawn system with Outback mix.

### Sprint 2 (Week 2)
- Add Eucalyptus biome tileset + fog overlay.
- Add wallaby and falling-branch hazards; Bilby Burrow power-up.
- Add weather events (wind, rain, heat haze) with UI indicators.
- Hook daily missions prototype + cosmetic shop stub (non-monetized).

## Technical Notes
- **Camera & Physics**: Fixed timestep; design for 60 FPS; parallax backgrounds on separate layer; collision bounds slightly inset to encourage flow.
- **Data-Driven Spawns**: Biome JSON/plist defining spawn weights per distance band; weather event scheduler with cooldown.
- **Audio**: Layered chiptune + didgeridoo bass; FM-synth magpie calls; configurable mix levels.
- **Build Targets**: iOS/Android using a cross-platform engine (e.g., Unity/Flutter+Flame); asset atlases sized for 1x/2x/3x.

## Coding the MVP (yes, we can build it)
- **Engine pick**: Unity (C#) or Flutter + Flame (Dart) both ship to iOS/Android quickly. Unity wins for built-in 2D physics and timeline tools; Flame is lighter if you prefer Dart.
- **Project layout (Unity)**:
  - `Scripts/Systems`: `GameLoop`, `Spawner`, `PowerUpManager`, `EventScheduler`.
  - `Scripts/Player`: `RooController`, `RooRush`, `CollisionHandler`.
  - `Scripts/UI`: `HUDController`, `PauseMenu`, `MissionPanel`.
  - `Resources/Data`: JSON for pace bands, spawn weights, missions.
- **Starter loop pseudocode (Unity-ish)**:
  ```csharp
  void Update() {
    if (!isAlive) return;
    HandleInput();           // tap/hold/double-tap → jump or Roo Rush
    ApplyGravity();          // tune gravity per pace band
    MoveForward(scrollSpeed);
    CheckCollisions();       // obstacles, power-ups
    spawner.Tick(Time.deltaTime, paceBand);
    events.Tick(Time.deltaTime, distance);
    hud.UpdateHUD(score, combo, powerUpTimer);
  }
  ```
- **Input mapping**: Tap = small impulse; hold 0.15–0.45s = scaled impulse; double-tap within 0.3s = Roo Rush state that ignores breakable collisions for ~0.4s.
- **Spawner stub**: Every 0.8–1.1s, roll against biome weights. Use an object pool with 8–12 instances per obstacle type; recycle offscreen.
- **Tuning data**: Load JSON at start so designers can tweak without rebuild. Example keys: `paceBands`, `spawnMix`, `powerUpDrop`, `eventWeights`.
- **Art placeholder pass**: Use colored rectangles and simple circles for greybox; replace with pixel sprites once feel is locked. Keep collision boxes slightly smaller than sprite silhouettes.
- **Build & test**: Create a CI job that builds iOS (Xcode archive) and Android (Gradle) nightly; auto-attach a tuning snapshot to the build artifact for QA.

## Prototype & Testing Approach
- **Greybox feel test (Day 1–2)**: Use engine primitives (no art) with 3 lane heights and 2 obstacle prefabs. Tune tap timing until neutral cadence sits near 140 BPM; verify Roo Rush window is readable.
- **Daily 10-minute playtest script**: (1) cold start → first 3 runs, (2) focus run to 800m, (3) mission check. Capture frustration points, visibility issues, and perceived fairness.
- **Metrics to log**: Distance histogram, time-to-first-hit, % runs using Roo Rush, power-up pickup rate, average combo tier reached, deaths by obstacle type.
- **Device checks**: Low-end Android (jitter/heat), small iPhone (thumb reach), tablet (UI scaling). Lock to 60 FPS; alert if frame drops >5% of frames.
- **Build pipeline**: Nightly ad-hoc/TestFlight build with versioned tuning tables. Keep a “paper build” (design doc + spawn sheets) updated alongside.

## Rough Screen Layout (retro mock)
- **HUD**: Score/combo top-left (pixel font, 8–16 px stroke). Power-up timer badge top-right; pause in top corner with CRT-style border.
- **Playfield**: Parallax background (3 layers), midground obstacles, foreground dust trail. Player sits ~35% from left edge with 2.5-tile lookahead.
- **Event cues**: Wind/rain icons at top-center; palette shift overlay. Ground line uses warm ochre; sky gradient to eucalyptus green or dusk purple.
- **Color palette swatches**: Rust red `#B04030`, ochre `#D89C42`, eucalyptus green `#5F8E73`, ocean teal `#2C8C8C`, dusk purple `#46345C`.
- **ASCII wireframe (landscape)**:
  ```
  [Score 123  x3]                           [Shield 07s]
  ------------------------------------------------------ sky parallax
                     ~  ~   (seagull)                    cloud layer
           (magpie)        Skippy>  o   {gumdrop}        midground
  ____  _    _   _   __  ____     ___    ____     __     ground tiles
  ````````````````````````````````````````````````````   HUD vignette
  ```

## Success Metrics (early soft launch)
- D1 retention 35%+, D7 12%+.
- Median session length 70–120s by day 3.
- Ad opt-in >20% of runs; cosmetic conversion 2–4% with soft currency focus.
