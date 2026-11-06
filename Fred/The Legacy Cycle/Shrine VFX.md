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

## Two versions, side by side (pick one)

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
- Each shrine has a temporary `TMP_CompareLabel` under its base. **Once a version is chosen**, delete the other model and the label. If B wins, rename `ShrineOfEmbers_Flipbook` to `ShrineOfEmbers`.

## Animation (`StarterPlayerScripts.ShrineFx`)

One client script drives every model tagged `ShrineFx`:
- **Flicker:** the light's brightness follows `math.noise`.
- **Breath:** every 5.5 s the coals brighten by about 25%, with a slow flicker.
- **Near:** within 12 studs, the fire and light stir by about 20%.
- **Use:** triggering either of the shrine's prompts gives a small puff of 14 sparks and 4 licks, and the fire rises 60% for 0.7 s. Only the player who used it sees the puff.
- **Low specs:** half the particles, no smoke, and no light shadows.
