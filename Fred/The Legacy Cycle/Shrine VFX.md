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
- **Breath:** every 5.5 s the fire swells by about 15%.
- **Near:** within 12 studs, the fire and light flare by about 40%. Measured: flame rate 36 to 51, light 1.55 to 1.93.
- **Use:** triggering either of the shrine's prompts emits 40 embers and 20 flames, and flares the fire for 0.7 s. Only the player who used it sees the burst.
- **Low specs:** half the particles, no smoke, and no light shadows.
