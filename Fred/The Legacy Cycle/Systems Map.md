---
tags: [legacy-cycle, architecture]
updated: 2026-11-09
---

# Systems Map

The Studio modules, grouped by where they run. Back to [[The Legacy Cycle]].

## ReplicatedFirst (client, first to run)

- `MainMenu`: the menu before the game, and the one Back to Main Menu reopens. See [[Join Flow]].
- `LoadingScreen` (LocalScript): the camera pan, the loading bar, then forge or continue. See [[Join Flow]].

## ReplicatedStorage (shared)

- `Appearance`: hero presets, plus `sanitize` and `apply`. See [[Character Creator]].
- `HeroParts.Hair`: the generated hair accessories.
- `Settings`: the settings values, `sanitize` and the client `watch`. See [[Settings]].
- `Ornament`, `UiFocus`: the UI kit and the screen takeover. See [[UI Kit (Ornament)]].
- `Combat.Config`: combat, movement, traversal, slide, sheathe and camera tuning, plus the animation ids and sound ids.
- `Combat.Theme`: colours, fonts and panel, bar and diamond helpers.
- `Townsfolk`: the 12 townsfolk (persona, look, stops, lines), the lore the AI may use, `pick` and `choices`. See [[Townsfolk]].
- `Packages.Trove`: Sleitnick's Trove v1.8.0 (MIT), which tracks connections, instances, threads and cleanup functions and cleans them all at once. Use it in new code that has to clean up. `CombatClient.Wind`, `Rocks` and the wall climb use it.
- `Stats`, `Progression`, `Quests`: shared definitions and `sanitize` for player data (see [[Decisions]], Stats and leveling).
- **Remotes:** `AppearanceAction`, `SettingsAction`, `StatAction`, `MenuAction` (the floating menu's open page), `EmberAction` (the absorb animation cue), `TownsfolkAction` (RemoteFunction: open, ask, bye), `TownsfolkReact` (a bump or blow line to show).

## ServerScriptService

- `Progression.Main` starts StatService, AppearanceService, SettingsService, EmberService, Leveling, QuestService and then PlayerData, so that no listener misses a player's first load.
- `Progression.PlayerData`:
  - One DataStore record per player: level, XP, stats, look, settings, ember, quest and done. DATA_VERSION is 3; `ember` is an added field, and older saves read it as false.
  - It autosaves every 120 s, and on leave and on shutdown.
  - If a load fails, that session is never saved.
- `Progression.AppearanceService`: see [[Character Creator]].
- `Progression.SettingsService`: see [[Settings]].
- `Progression.EmberService`: the once-per-player "Absorb the Ember" prompt on every `Shrine`-tagged part. It also publishes the `HasEmber` attribute. See [[Shrine VFX]].
- `MenuPresence`: relays each player's open floating-menu page as the `MenuPage` attribute. See [[Floating Menu]].
- `Progression.StatService`, `Leveling`, `QuestService`: stat points, XP and level-ups, and quests.
- `Combat.CombatService`: server combat; it clones the katana onto characters. Its `ClimbStart` and `ClimbEnd` actions call `CombatCore.climb`, which drains stamina while the climber moves and stops regen while they're on the wall.
- `Combat.EnemyAI`: enemies, cloned from templates.
  - A Hollow Squire spawns at every part tagged `EnemySpawn`: the combat sandbox, and the dev space at (-130, 2990).
  - A Training Dummy stands at every part tagged `DummySpawn` (three in `workspace.Dev.Dummies`). Dummies have no brain and an anchored root, 1000 HP and a full heal 3 s after the last hit. They respawn after death and carry `Dummy = true`, which Leveling and QuestService skip, so they give no XP or kill credit.

- `Townsfolk` (Script): spawns the townsfolk, walks their routines and runs conversations. Its `UseAI` attribute switches the AI off. `Townsfolk.Mind` asks TextGenerator in character, with written lines as the fallback. See [[Townsfolk]].

## StarterPlayerScripts (client)

- `CombatClient` and its modules:
  - `CombatCamera`: a third-person shoulder camera, lock-on, spring kicks and speed FOV. It skips its update while the camera is Scriptable.
  - `Feedback`: camera shake, hitstop, sounds (through the Effects group), sparks, blade trails and the hurt vignette.
  - `Locomotion`: procedural lean, banking, head look and afterimages.
  - `Traversal`: double jump, wall run, wall climb (up, down and sideways, held by a LinearVelocity), mantle and vault.
  - `Rocks`: the big-drop ground impact. `Rocks.impact(root, k, fromAbove?)` reads the ground's material, and `Rocks.kindOf` maps it to one of 9 profiles in `PROFILES` (stone, earth, mud, sand, snow, ice, wood, metal, water). The profile sets the rim pieces, the debris shape and amount, the dust or spray, and any sparks, all in the ground's own material and colour. Everything sinks back after about 2.3 s and is cleaned by one Trove. It runs only on the client, so you only see your own.
  - `Wind`: wind VFX. `ring` is the shockwave for double jumps, wall kicks and ground-pounds, `burst` is the double-jump updraft, and `dash` is the dodge slipstream (through `Locomotion.dash`). They are pale curved-Beam gusts with a puff of air, and each effect cleans up through its own Trove.
  - `FloatMenu`: the M menu. See [[Floating Menu]].
  - `ReturnToMenu`: the camera-rise animation for Back to Main Menu.
  - `Vitals`: the health, stamina and (once absorbed) Ember arcs.
  - `LockReticle`.
  - `MobileLayout`: touch buttons arranged in arcs around the jump button.
- `AppearanceClient`: holds the `Creator` module (opened by the join flow and the main menu). See [[Character Creator]].
- `DialogueClient`: Captain Aldric's scripted talk, and townsfolk reply choices (the AI notice shows only if `UseAI` is on).
- `TownsfolkBarks`: remarks and bump/blow reactions from townsfolk, drawn by its `Bubble` module.
- `ShrineFx`: the shrine fire animation, the absorb animation and the dark state. See [[Shrine VFX]].
  - `ShrineFx.Cinematic`: the absorb cutscene's camera and effects: `take()` (bars, grade, depth of field, bloom, flash, FOV punch, cleanup), `dolly` and `orbit` shots, and `ring`, `pillar` and `motes`.

## Workspace and SoundService

- `Workspace.Cinematics.LoadingPan`: the shot folders for the loading pan.
- `Workspace.World.Havenbrook.Townsfolk`: `Stops` (22 invisible stop parts, found by searching for clear ground) and `People` (filled at run time).
- `SoundService.Music`: holds World.Ambience.
- `SoundService.Effects`: holds the combat sound templates and the FountainSound.
