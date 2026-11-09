---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Character Creator

Players build their hero from presets. The look is saved as 6 indices and applied to the character on the server. Back to [[The Legacy Cycle]]. The decisions behind it are in [[Decisions]].

## Presets: `ReplicatedStorage.Appearance`

- `Look = {skin, face, hair, hairColor, outfit, hair2}` (all indices). The default is `{3, 10, 13, 2, 2, 1}`: Confident face, Style 12, no secondary hair.
- **Skins:** 8 tones, from rgb(243,217,196) to rgb(63,38,22).
- **Faces:** 61 decals from the user's face pack (63 parts, two duplicate textures dropped), sorted by name.
  - `apply` clears the StarterCharacter head's TextureID (its stock face) and welds an invisible `HeroFace` plate (1.1 × 1.1 × 0.04) just in front of the head, with the decal on it.
  - The plate is needed because a decal on a MeshPart follows the mesh's UVs and smears across the head.
  - The dynamic head's eye sockets still show faintly under the decal.
- **Hairs:** Shorn plus Style 1–33, from the user's hair pack. Each is an Accessory `Hair01`–`Hair33` in `ReplicatedStorage.HeroParts.Hair`, with the mesh tinted by the hair colour.
  - Each carries `Source` (mesh id) and `Fit` (scale, dy, dz) attributes. Most hairs were scaled 1.04–1.36× so the head doesn't poke through. Style 11 still shows a small scalp gap on top.
- **Secondary hair:** `hair2` uses the same list and the same colour, and is worn on top of the main style (a fringe, a bun). 1 means none.
- **Hair colours:** 7, each with a name.
- **Outfits:** Ashen, Ember, Moss and Ironbound, each a top and bottom colour. The belt and boots are fixed.
- **Functions:**
  - `sanitize(data)`: strict integer range checks; returns nil if any field is bad. A missing `hair2` counts as 1, so saves from before it existed still load.
  - `apply(model, look)`: colours the parts, swaps in tinted copies of both hairs and paints the face.
  - There is a self-check at the bottom of the module.
- The old generated hairs (Short, Messy, Long, Tied) are kept in `ServerStorage.OldHeroHair`. The two packs are kept in `ServerStorage.ContentPacks`.

## Server: `ServerScriptService.Progression.AppearanceService`

- When a profile loads, it publishes the `HasLook` attribute (false until a hero exists) and the `Look` attribute (the indices, comma-joined, `hair2` last).
- It dresses every spawned character in the saved look, or in the default if there isn't one.
- `AppearanceAction:FireServer("Save", look)` is checked with sanitize, limited to one save a second, stored in PlayerData `look` and applied to the character again.
- The Shrine of Embers no longer has a "Change appearance" prompt (removed 2026-11-09, user). The creator opens from the join flow and the main menu.

## Client: `StarterPlayerScripts.AppearanceClient` and its `Creator` module

- Faces are decals now, so the old per-frame FaceControls driver is gone.
- **`Creator.open({firstTime, onDone})`:**
  - Left side: a turnable ViewportFrame preview inside a rune circle. You can drag it, or use the chevron buttons, which are drawn from bars.
  - Right side: tabs for Skin, Face, Hair and Outfit.
  - **Face:** 18 keycaps a page (4 pages), each showing the decal on a skin-coloured plate.
  - **Hair:** a Style row and a Secondary row, 6 keycaps a page each (6 pages), plus the colour swatches. Each keycap is a small ViewportFrame of the hair on the preview hero's head.
  - Pagers ("‹ 1 / 6 ›") sit at the right of each section label and open on the page holding the current pick.
  - Footer: Randomize (first time) or Cancel, plus "Begin your legacy" or "Save look". Randomize leaves the secondary hair off 70% of the time.
- The preview is a copy of the character stripped to its body parts and accessories, with the camera looking at `(0, 2.9, 0)` because a character's pivot is at its feet.

## Hair meshes

- The hair pack is the user's own (audited: no scripts).
- The older generated hairs came from `generate_mesh`, are owned by the place, and have the asset ids 132652460004873, 86446319474707, 122427027454983 and 131360266416617.
- Never use Creator Store hair models (see [[Studio Gotchas]]).
