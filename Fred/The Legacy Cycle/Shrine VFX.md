---
tags: [legacy-cycle, system, vfx]
updated: 2026-11-09
---

# Shrine VFX

The Shrine of Embers in Havenbrook now uses the user's imported obsidian-brazier mesh in place of the old blocky shrine. Back to [[The Legacy Cycle]]. The prompts it carries are described in [[Character Creator]].

## The model

- `Workspace.World.Havenbrook.ShrineOfEmbers`:
  - `Shrine`: the MeshPart, scaled from 1.9 to 6 studs tall and anchored. It is tagged `Shrine`, so EmberService gives it the B "Absorb the Ember" prompt, its only prompt (the R stat reset and V appearance prompts were removed on 2026-11-09, user).
  - `FxCore`: an invisible emitter volume in the bowl, at 70% of the height. It holds the emitters and the `Glow` PointLight (255,128,56, range 16, shadows on).
  - `FxSmoke`: an invisible part above the bowl for the smoke.
- The model is tagged `ShrineFx`. Its `FxVariant` attribute records which version it is.
- `ModelStreamingMode = Atomic`, so `FxCore` streams in with the model. Without it, ShrineFx gave up after 10 s and the shrine stayed still.

## Look: an ember, not a bonfire (2026-11-06)

The user said the first pass was too strong and that an ember should be a weak fire. Both versions are now a smoulder:
- Low licks: A's flame is rate 9 and size 0.7, and B's flipbook is rate 2.5 and size 1.5, fading out by 75% of its life.
- About 3 sparks a second and thin smoke.
- A dim red light: (255,96,40), brightness 0.9, range 9.
- The brazier's own sculpted flame now carries most of the look.

## Chosen: A · Native (2026-11-06)

The user picked A. B (the flipbook copy) and both comparison labels were deleted. The table below is kept as a record of what was compared. The uploaded flipbook image (`rbxassetid://124247210335986`) is still in the user's Roblox inventory, unused.

## Two versions compared

| | A · Native Roblox VFX | B · Blender flipbook |
|---|---|---|
| Where | `ShrineOfEmbers`, at (-205, 3048) | `ShrineOfEmbers_Flipbook`, 12 studs east |
| Fire | Two emitters using the built-in `fire_main.dds`: Flame (orange to red) and Core (white-hot) | One `Flame` emitter on `FxFlip`: an 8×8 flipbook (`rbxassetid://124247210335986`, OneShot, rate 7, size 3 to 3.6) |
| Embers, smoke, light | Built-in sparkles and smoke textures | The same as A |
| Look | A soft, fluffy fireball sitting on the coals | A taller flame column that rises and licks |

- The flipbook came from a Blender 5.2 Mantaflow sim in the scene `FX_ShrineFire`:
  - Domain 1.4 × 1.4 × 3, resolution 112, a 14-frame fuel burst.
  - 38 frames of life, stretched over 64 cells and rendered in EEVEE at 128 px per cell.
  - Alpha comes from brightness, with a threshold to remove render noise.
  - The working files are in this session's scratchpad (`shrine_fx`), which is temporary, so re-bake if the source is needed.

## Animation (`StarterPlayerScripts.ShrineFx`)

One client script drives every model tagged `ShrineFx`:
- **Flicker:** the light's brightness follows `math.noise`.
- **Breath:** every 5.5 s the coals brighten by about 25%, with a slow flicker.
- **Near:** within 12 studs, the fire and light stir by about 20%.
- **Low specs:** half the particles, no smoke, and no light shadows.

## Absorbing the Ember (2026-11-06)

The user asked for this. It follows the prologue script's Scene 6: the golden spark settles into the player's chest, and the Ember bar appears in the HUD, faint.

- **Rule:** you absorb it **once per player**, and it is saved. Each Shrine of Embers has a held **B** prompt (1.2 s, gamepad DPad-left) labelled "Absorb the Ember", the shrine's only prompt. `Progression.EmberService` sets PlayerData `ember = true` and the `HasEmber` attribute, then fires `EmberAction` `"Absorbed"` to that player.
- **Cinematic (user, 2026-11-09; reworked the same day because the user wanted it cooler):** about 6 s, in `ShrineFx` with the shots and effects in its `Cinematic` module. Letterbox bars close in, `UiFocus` hides the HUD and holds the hero still, and the hero turns to the shrine. The picture is graded warm and desaturated, with depth of field and bloom (both off in low specs). The 5 beats:
  1. **Anticipation (1 s):** an over-the-shoulder dolly toward the brazier (FOV 50 to 46). The world darkens and a low swing sounds.
  2. **Lighting the embers (1.7 s):** the camera orbits the shrine, rising. The fire roars up (`ignite`, +160% fire and light) with a warm flash, a FOV punch, a shake and a deep strike. A ring of fire runs out along the ground, a pillar of light stands up from the bowl, and 5 motes spiral up it in a helix (3 in low specs).
  3. **Collecting its power (2.6 s):** the camera orbits the hero from the front. The spark rises and hangs while the hero lifts 0.9 studs off the ground. Then 8 motes (4 in low specs) spiral into the chest as the spark arcs in.
  4. **Impact:** an impact star (two star particles of 0.05 s, one growing and one shrinking, `Cinematic.impact`; the ignition gets one too), a white-gold flash, a bigger punch and shake, a clash, a ring at the feet, an ember aura round the hero that beats twice (fill 0.25 to 0.6 and back, Circular) and then sinks away, and a shower of 45 sparks.
  5. **Recovery:** the camera pulls back to a wide three-quarter as the hero settles to the ground. The shrine goes out, and the floating menu opens on the Embers page and lights the skill tree (`FloatMenu.revealEmbers`, see [[Floating Menu]]).
  - **Particle curves** (from [[Roblox VFX Techniques]]): the spark particles grow fast then die away, hold their opacity before fading, and spin at random. The motes grow in over the first 15% of their path, and their trails hold before fading.
  - The hero's head no longer follows the camera while the camera is Scriptable (Locomotion), so the cutscene camera can't turn the head away from the shot.
  - It stops early (camera, bars, grade, HUD restored, no menu) if the hero dies or respawns.
  - **To test it, use the dev shrine:** `workspace.Dev.DevShrineOfEmbers` (`DevReplay = true`) replays the cinematic on B, with an 8 s cooldown. It never saves the Ember and relights itself afterwards.
- **The dark shrine:** no flame, embers or light, just a thin cold wisp of smoke. Its SurfaceAppearance is tinted to (72,66,64), which also turns the sculpted flame cold. This happens on the absorber's screen only; players who haven't taken their Ember still see the shrine lit. The absorb prompt is hidden locally once `HasEmber` is true, so a dark shrine has no prompt at all.
- **Elsewhere:**
  - Vitals has a faint Ember arc inside stamina (radius 67). It fills when the Ember arrives, then breathes. Shortcut: nothing drains it yet, so it always shows full.
  - The floating menu's Embers page reads "You have not found your Ember" / "Seek the Shrine of Embers in Havenbrook" until you have it, then "Your Ember burns", with a lit ember diamond and an ember-red light.
- **Not changed:** the Ember stat stays hidden until Echoes (a logged decision). Nothing spends the Ember yet (Ember Strike, saving Bryn).
