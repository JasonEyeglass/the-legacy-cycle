---
tags: [legacy-cycle, system, ui]
updated: 2026-11-06
---

# Character Creator

Players build their hero from presets. The look is saved as 5 indices and applied to the character on the server. Back to [[The Legacy Cycle]]. The decisions behind it are in [[Decisions]].

## Presets: `ReplicatedStorage.Appearance`

- `Look = {skin, face, hair, hairColor, outfit}` (all indices). The default is `{3, 1, 2, 2, 2}`.
- **Skins:** 8 tones, from rgb(243,217,196) to rgb(63,38,22).
- **Faces (FaceControls presets):**
  - Calm.
  - Stern.
  - Weary.
  - Bold, a lopsided smirk.
  - Stern and Weary look close to Calm, because the head has no eyebrows.
- **Hairs:** Shorn (bald), Short, Messy, Long and Tied. Each is an Accessory in `ReplicatedStorage.HeroParts.Hair`, made from a generated MeshPart. TextureID is cleared so its Color can tint it.
- **Hair colours:** 7, each with a name.
- **Outfits:** Ashen, Ember, Moss and Ironbound, each a top and bottom colour. The belt and boots are fixed.
- **Functions:**
  - `sanitize(data)`: strict integer range checks; returns nil if any field is bad.
  - `apply(model, look)`: colours the parts, swaps in a tinted copy of the hair and sets the face.
  - `setFace`.
  - `faceFromAttribute`.
  - There is a self-check at the bottom of the module.

## Server: `ServerScriptService.Progression.AppearanceService`

- When a profile loads, it publishes the `HasLook` attribute (false until a hero exists) and the `Look` attribute (the indices, comma-joined).
- It dresses every spawned character in the saved look, or in the default if there isn't one.
- `AppearanceAction:FireServer("Save", look)` is checked with sanitize, limited to one save a second, stored in PlayerData `look` and applied to the character again.
- Every Shrine-tagged part gets a **LookPrompt**: "Change appearance", key V (gamepad DPadDown), sitting under the respec prompt. Triggering it sends `FireClient("Open")`.

## Client: `StarterPlayerScripts.AppearanceClient` and its `Creator` module

- A live character's Animator resets FaceControls every frame. So `BindToRenderStep("HeroFaces", Last)` re-applies every player's face from their `Look` attribute.
- **`Creator.open({firstTime, onDone})`:**
  - Left side: a turnable ViewportFrame preview inside a rune circle. You can drag it, or use the chevron buttons, which are drawn from bars.
  - Right side: tabs for Skin, Face, Hair and Outfit.
  - Footer: Randomize (first time) or Cancel, plus "Begin your legacy" or "Save look".
- The preview is a copy of the character stripped to its body parts and accessories, with the camera looking at `(0, 2.9, 0)` because a character's pivot is at its feet.

## Hair meshes

- They came from `generate_mesh`, are owned by the place, and were fitted on a dummy.
- Asset ids: 132652460004873, 86446319474707, 122427027454983, 131360266416617.
- Never use Creator Store hair models (see [[Studio Gotchas]]).
