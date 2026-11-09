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
- `Items`: every item (id, name, slot, icon, power, stack, blurb) and the inventory rules: `add`, `remove`, `equip`, `power`, `sanitize`, and `selfTest()` to run from the command bar. Item ids are save keys, so never rename one.
- **Remotes:** `AppearanceAction`, `SettingsAction`, `StatAction`, `InventoryAction` ("Equip", itemId), `MenuAction` (the floating menu's open page), `EmberAction` (the absorb animation cue), `TownsfolkAction` (RemoteFunction: open, ask, bye), `TownsfolkReact` (a bump or blow line to show), `StoryChoice` (conversation, option).

## ServerScriptService

- `Progression.Main` starts StatService, AppearanceService, SettingsService, EmberService, Leveling, QuestService, InventoryService, StoryService and then PlayerData, so that no listener misses a player's first load.
- `Progression.PlayerData`:
  - One DataStore record per player: level, XP, stats, look, settings, ember, quest, done, gold, inv and story. DATA_VERSION is 4. Fields added later (`ember`, then `gold`, `inv` and `story`) load as false or empty from older saves.
  - `validFlag(flag, value)` says whether a story flag fits the save.
  - It autosaves every 120 s, and on leave and on shutdown.
  - If a load fails, that session is never saved.
- `Progression.AppearanceService`: see [[Character Creator]].
- `Progression.SettingsService`: see [[Settings]].
- `Progression.EmberService`: the once-per-player "Absorb the Ember" prompt on every `Shrine`-tagged part. It also publishes the `HasEmber` attribute. See [[Shrine VFX]]. It also makes every `EmberLantern`-tagged model (a `Flame` part with `Sparks`, and a `Touch` prompt with Dialogue `EmberLantern`) flare for anyone with an Ember. There's one by Havenbrook's quest board and a test copy in the dev space.
- `MenuPresence`: relays each player's open floating-menu page as the `MenuPage` attribute. See [[Floating Menu]].
- `Progression.StatService`, `Leveling`, `QuestService`: stat points, XP and level-ups, and quests. StatService multiplies damage by the equipped weapon's power. QuestService also:
  - starts story quests (`startsWithEmber`) by themselves;
  - pays `rewardGold` through InventoryService;
  - exposes `talked(player, npc)`, which the Townsfolk script calls when a conversation opens.
  - `abandon(player)`, which the quest scroll's "Abandon quest" sends through `QuestEvent("Abandon")`. A story quest then starts over.
- Quests so far: `ASpark` (1.1 A Spark in Havenbrook: Orla, the lantern, Orla; 60 XP and 100 gold), `WoundedKnight` (1.3: Bryn's choice, then Orla; 80 XP; starts after 1.1) and `MissingCaravan` (Aldric). A `choose` step waits for its story flag (`QuestService.waitingOn` and `flagSet`).
- `Progression.InventoryService`: `add`, `remove`, `has`, `equip` and `addGold` for a player. It replicates `Gold`, `Inventory` (JSON) and `Equip_<slot>` attributes, and gives every hero `Items.Starter`, equipped.
- `Progression.StoryService`: `get` and `set` for story flags (`knight_fate`, `prologue_seen`, `act1_complete`), replicated as `Story_<flag>` attributes. It also handles the `StoryChoice` remote: a scripted conversation's choice (only `BrynHale` so far), checked against the quest, the flag and the distance to the speaker.
- `Combat.CombatService`: server combat; it clones the katana onto characters. Its `ClimbStart` and `ClimbEnd` actions call `CombatCore.climb`, which drains stamina while the climber moves and stops regen while they're on the wall.
- `Combat.EnemyAI`: enemies, cloned from templates.
  - An enemy spawns at every part tagged `EnemySpawn`. Its `Enemy` attribute picks the template (Hollow Squire by default, Hollowed Soldier, Void Hound, Void Brute), and `Config.Enemy.Kinds` tunes each kind: telegraphs, heavy chance, reach, damage, poise, packs and hit-and-run backing off. Spawns: the combat sandbox; the dev space at (-130, 2990) (Squire), (-60, 2990) (soldier), (-60, 2910) (3 hounds) and (-150, 2900) (brute).
  - A Training Dummy stands at every part tagged `DummySpawn` (three in `workspace.Dev.Dummies`). Dummies have no brain and an anchored root, 1000 HP and a full heal 3 s after the last hit. They respawn after death and carry `Dummy = true`, which Leveling and QuestService skip, so they give no XP or kill credit.

- `Nameplate` (ModuleScript): the game's overhead nameplate, `attach(head, name, title?, maxDistance?, hideFrom?)`. It also turns off Roblox's own name on that Humanoid. Used by `KnightGuard` and `PlayerNameplates`; townsfolk build the same look in `Townsfolk`.
- `PlayerNameplates` (Script): each player's display name over "Level N", hidden from the player themself.
- `KnightGuard` (Script): every model tagged `KnightGuard` gets a nameplate ("Havenbrook Guard"), loops the `KnightIdle` animation and plays a random fidget (`KnightIdleInspect`, `Spin`, `Shoulder`, `GroundTap`) every 5–11 s at Movement priority. Animations load through `Combat.AnimLoader`. The Havenbrook Knight is tagged.

- `Townsfolk` (Script): spawns the townsfolk, walks their routines and runs conversations. Its `UseAI` attribute switches the AI off. `Townsfolk.Mind` asks TextGenerator in character, with written lines as the fallback. See [[Townsfolk]].

## StarterPlayerScripts (client)

- `CombatClient` and its modules:
  - `CombatCamera`: a third-person shoulder camera, lock-on, spring kicks and speed FOV. It skips its update while the camera is Scriptable.
  - `Feedback`: camera shake, hitstop, sounds (through the Effects group), sparks, blade trails and the hurt vignette, and the crescent slash (`Feedback.slash(character)`: finds the swing track, then stacks the four `Config.SlashFx` flipbooks on each measured cut in `Config.SlashFx.Cuts` as the animation reaches it; pooled rigs in `Workspace.Terrain` named `SlashFx_<kind>`).
  - `Locomotion`: procedural lean, banking, head look and afterimages.
  - `Traversal`: double jump, wall run, wall climb (up, down and sideways, held by a LinearVelocity), mantle and vault.
  - `Minimap`: the radar (top right), turning with the camera. It shows `MapLandmark` anchors with labels, enemies as red wax dots, `KnightGuard` guards as ink shields (`icon_guard`), and the quest star pinned to the edge.
  - `ZoneTitle`: writes a zone's name at the top centre on entering it. Zones are `MapLandmark`s with a `Radius` attribute.
  - `Rocks`: the big-drop ground impact. `Rocks.impact(root, k, fromAbove?)` reads the ground's material, and `Rocks.kindOf` maps it to one of 9 profiles in `PROFILES` (stone, earth, mud, sand, snow, ice, wood, metal, water). The profile sets the rim pieces, the debris shape and amount, the dust or spray, and any sparks, all in the ground's own material and colour. Everything sinks back after about 2.3 s and is cleaned by one Trove. It runs only on the client, so you only see your own.
  - `Wind`: wind VFX. `ring` is the shockwave for double jumps, wall kicks and ground-pounds, `burst` is the double-jump updraft, and `dash` is the dodge slipstream (through `Locomotion.dash`). They are pale curved-Beam gusts with a puff of air, and each effect cleans up through its own Trove.
  - `FloatMenu`: the M menu. See [[Floating Menu]].
  - `ReturnToMenu`: the camera-rise animation for Back to Main Menu.
  - `Vitals`: the health, stamina and (once absorbed) Ember arcs.
  - `LockReticle`.
  - `MobileLayout`: touch buttons arranged in arcs around the jump button.
- `AppearanceClient`: holds the `Creator` module (opened by the join flow and the main menu). See [[Character Creator]].
- `DialogueClient`: scripted talk (Aldric, the lantern, Bryn), story choices at the end of a scripted conversation, and townsfolk reply choices (the AI notice shows only if `UseAI` is on). A prompt with `NoTurn` keeps its speaker still.
- `StoryWorld`: shows a `StoryNPC` model only to players whose active quest is its `ShowQuest`, and shows the player's `FateFlag` choice on it (Bryn: the Void put out, or his sword gone).
- `TownsfolkBarks`: remarks and bump/blow reactions from townsfolk, drawn by its `Bubble` module.
- `ShrineFx`: the shrine fire animation, the absorb animation and the dark state. See [[Shrine VFX]].
  - `ShrineFx.Cinematic`: the absorb cutscene's camera and effects: `take()` (bars, grade, depth of field, bloom, flash, FOV punch, cleanup), `dolly` and `orbit` shots, and `ring`, `pillar` and `motes`.

- `ReplicatedStorage.Audio` (client): music crossfades (`music`), one-shot UI sounds (`ui`), and `start`, which gives every PlayerGui button hover and click sounds (a `UiSound` attribute of "Select" or "None" overrides) and plays Select on ProximityPrompts.

## Workspace and SoundService

- `Workspace.Cinematics.LoadingPan`: the shot folders for the loading pan.
- `Workspace.World["Old Watchtower"]` and `Workspace.World["Ser Bryn Hale"]` (tagged `StoryNPC`, anchored, posed sitting; Persistent streaming): quest 1.3.
- `Workspace.World.Havenbrook.Townsfolk`: `Stops` (22 invisible stop parts, found by searching for clear ground) and `People` (filled at run time).
- `SoundService.Music` (SoundGroup, the Music slider): BGM-1 (main menu, low under the loading screen) and BGM-2 (gameplay), played by `ReplicatedStorage.Audio`.
- `SoundService.Effects` (SoundGroup, the Sound effects slider): holds the combat sound templates, the FountainSound, and `UI`: the UI sounds (click, hover, open, select, mission, quest complete, quest failed).
