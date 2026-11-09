---
tags: [legacy-cycle, system, vfx]
updated: 2026-11-06
---

# Shrine VFX

The Shrine of Embers in Havenbrook now uses the user's imported obsidian-brazier mesh in place of the old blocky shrine. Back to [[The Legacy Cycle]]. The prompts it carries are described in [[Character Creator]].

## The model

- `Workspace.World.Havenbrook.ShrineOfEmbers`:
  - `Shrine`: the MeshPart, scaled from 1.9 to 6 studs tall and anchored. It is tagged `Shrine`, so StatService and AppearanceService still give it the R "Reset stat points" and V "Change appearance" prompts.
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
- **Use:** triggering either of the shrine's prompts gives a small puff of 14 sparks and 4 licks, and the fire rises 60% for 0.7 s. Only the player who used it sees the puff.
- **Low specs:** half the particles, no smoke, and no light shadows.

## Absorbing the Ember (2026-11-06)

The user asked for this. It follows the prologue script's Scene 6: the golden spark settles into the player's chest, and the Ember bar appears in the HUD, faint.

- **Rule:** you absorb it **once per player**, and it is saved. Each Shrine of Embers has a held **B** prompt (1.2 s, gamepad DPad-left) labelled "Absorb the Ember", shown above the R and V prompts. `Progression.EmberService` sets PlayerData `ember = true` and the `HasEmber` attribute, then fires `EmberAction` `"Absorbed"` to that player.
- **Cinematic (user, 2026-11-09):** the absorb now plays as a short cutscene of about 5 s. Letterbox bars close in, `UiFocus` hides the HUD and holds the hero still, and a Scriptable camera turns the hero toward the shrine.
  - **Shot 1, lighting the embers (1.9 s):** a low three-quarter push-in on the coals (FOV 48 to 42). The smoulder roars up through an `ignite` value (+160% fire and light), with a burst of embers and a column of rising sparks.
  - **Shot 2, collecting its power:** hero and shrine side-on, drifting in (FOV 55 to 52), while steps 1 to 4 below play.
  - The camera comes back 0.7 s into the shrine going out. The floating menu then opens on the Embers page and lights the skill tree (`FloatMenu.revealEmbers`, see [[Floating Menu]]).
  - It stops early (camera, bars and HUD restored, no menu) if the hero dies or respawns.
  - **To test it, use the dev shrine:** `workspace.Dev.DevShrineOfEmbers` (`DevReplay = true`) replays the cinematic on B, with an 8 s cooldown. It never saves the Ember and relights itself afterwards.
- **Animation (client, about 3.7 s, `ShrineFx`):**
  1. The fire flares one last time while a neon spark (with a trail, motes and a light) grows and rises out of the coals.
  2. It hangs there, beating, while the fire starts to fail.
  3. It arcs into the chest, speeding up.
  4. A flash, a shower of 30 sparks (14 in low specs), a 0.25 shake and a sound.
  5. The shrine goes out.
- **The dark shrine:** no flame, embers or light, just a thin cold wisp of smoke. Its SurfaceAppearance is tinted to (72,66,64), which also turns the sculpted flame cold. This happens on the absorber's screen only; players who haven't taken their Ember still see the shrine lit. The absorb prompt is hidden locally once `HasEmber` is true. The R and V prompts still work, with no spark puff while the shrine is dark.
- **Elsewhere:**
  - Vitals has a faint Ember arc inside stamina (radius 67). It fills when the Ember arrives, then breathes. Shortcut: nothing drains it yet, so it always shows full.
  - The floating menu's Embers page reads "You have not found your Ember" / "Seek the Shrine of Embers in Havenbrook" until you have it, then "Your Ember burns", with a lit ember diamond and an ember-red light.
- **Not changed:** the Ember stat stays hidden until Echoes (a logged decision). Nothing spends the Ember yet (Ember Strike, saving Bryn).
