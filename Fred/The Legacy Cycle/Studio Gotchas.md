---
tags: [legacy-cycle, gotchas]
updated: 2026-11-06
---

# Studio Gotchas

Lessons from building The Legacy Cycle through Roblox Studio and the Studio MCP. Back to [[The Legacy Cycle]].

## Engine

- **The camera:** the default camera takes a Scriptable camera back when the character spawns. A cinematic has to set Scriptable again every frame.
- **Faces:** a live character's Animator resets FaceControls every frame. Re-apply expressions in `BindToRenderStep(..., RenderPriority.Last)`. An unanimated ViewportFrame copy keeps them.
- **Streaming:** `RequestStreamAroundAsync` works from a LocalScript, but it wears off. Re-request every ~2 s, and put the marker parts in a Persistent model.
- **The viewport:** it is tiny for a moment at startup. Layout code should skip any frame under 200×150.
- **Pivots:** a character's pivot is at its feet; the HumanoidRootPart is 3 studs higher.
- **Touch buttons:** ContextActionService makes at most 7 of them. Any further buttons silently get none.
- **First person:** the mouse is locked to the centre of the screen. A world-space menu has to keep still so the player can aim at it by looking (see [[Floating Menu]]).
- **Rotated frames are not antialiased.** A curve built from rotated rectangles looks jagged. Use a round frame (`UICorner` at 1, 0) with a `UIStroke`, which is smooth. Cut it to an arc with a `ClipsDescendants` frame, which is exact; a `UIGradient` mask on the stroke spans a bigger box than the frame and leaks past the arc. A `Border` stroke grows outward from the frame's edge. (Vitals, 2026-11-06.)
- **First person hides your own body** through `LocalTransparencyModifier`. A Scriptable camera never resets it, so a cutscene that starts in first person shows no hero. Set the modifier to 0 every frame until the default camera is back (ReturnToMenu).
- **`TextScaled` with a `UITextSizeConstraint` ignores `UIScale`:** the cap is in screen pixels, so text on a scaled-up stage comes out small. On a UIScale'd stage, use a fixed `TextSize`, which does scale.
- **A Decal on a MeshPart follows the mesh's UVs**, so a classic face decal on the dynamic head smears. Put it on an invisible box Part welded in front of the face instead.
- **ViewportFrames can capture blank** in `screen_capture` even with their contents in place. Nudging the camera or ImageTransparency redraws them.
- **CanvasGroup `GroupTransparency` was ignored in playtests** (2026-11-09): both a 1,209 px strip and a 24 px mark stayed fully drawn at 1. Fade the pieces one by one instead (text, image and stroke transparencies).
- **A custom shift lock** sets `MouseBehavior = LockCenter` and `UserGameSettings.RotationType = CameraRelative` every frame from a render step after the camera, which resets both.
- **Our own head-look follows the camera** (Locomotion): the local hero's head turns toward where the camera looks, so a cutscene camera beside the hero turned the head away from the shot. It now skips the local hero while the camera is Scriptable.
- **A line that starts with `(`** continues the line before it as a function call ("Ambiguous syntax"). End the earlier line with `;`.

## Asset safety

- Three free "No scripts" Creator Store hair models each hid a Script, a LocalScript, a RemoteFunction and a 400-piece ScreenGui under `Handle.AvatarPartScaleType.Runtime`. They ran only in Studio, and showed a fake "procedure + code sample" overlay aimed at tricking developers. They were deleted.
- After every `insert_asset`, count the inserted parts by class. One "ponytail" search result was actually a Sky.

## Studio MCP testing

- **Edit vs Play:** edits only apply in Edit mode. `multi_edit` can report "Target is not reachable" right after Play stops and still apply, so read the source before retrying.
- **Module instances:** `execute_luau` (Client or Server) gets its own module instances, not the game's.
- **Clicking GUI buttons:** `user_mouse_input` clicks don't reach GUI buttons. The workaround is to set `GuiService.SelectedObject = button` from `execute_luau` (Client), then `user_keyboard_input` a `Return` key press. That fires Activated, and it works for ScreenGuis and the PlayerGui SurfaceGui. The button has to be visible and finished fading.
- **The main menu blocks the game until Play.** To test in-game systems, press Play through the selection trick above.
- **Screenshots:** the first capture after moving the camera can be stale. Set the camera twice with a wait between.
- **The Assistant undoes camera changes:** after an `execute_luau` that sets `CameraType`, Studio's Assistant resets it ("The execute_luau changed camera type. Resetting..."). To hold a test camera, set it every frame from a `BindToRenderStep` at `Last`, and unbind it afterwards.
- **Captures time out during heavy scenes:** a `screen_capture` mid-cinematic can time out or return a blank frame, and the MCP connection can drop for a few seconds. Check state with `execute_luau` before trusting a capture, and hold the shot as a still to inspect it.
- **Clones keep joints that point outside them.** `ServerStorage.Combat.Katana`'s `RightHandle.Motor6D` has Part0 = `Workspace.Animations.Katana Rig`'s Right Arm, so parenting a clone of the katana into Workspace snapped that rig onto the clone (2026-11-09). Before placing a clone, check every JointInstance, constraint and attachment reference inside it, and destroy or repoint any whose Part0, Part1 or Attachment is outside the clone.

## Particles

- **A ParticleEmitter's `Size` is half the card's rendered width.** A sheet whose arc radius is 0.369 of a cell needs `Size = radius / 0.369 / 2` (measured 2026-11-09).
- **`VelocityPerpendicular` cards:** the card lies flat across the velocity v. Its unrotated image-up is world up projected onto the card, and image-right is v × up. `Rotation` +θ turns the image by −θ about v. Near-vertical v (a flat sweep) uses a fallback up, so tip v a few degrees off vertical.
- **Edit-mode particle captures:** emit with a short lifetime, wait about half a second inside the same `execute_luau`, then capture. A freshly set texture can show nothing on the first emit while it loads; emit again.

- **Posing an animation in Edit mode:** load it on a rig's Animator, `track:Play(0, 1, 1)`, then call `animator:StepAnimations(1/60)` in a loop. That matches Play mode exactly. Setting `TimePosition` and stepping 0 does not pose it correctly.
- **The Katana Rig in `Workspace.Animations` is R6** (its katana hangs off "Right Arm"). The game is R15, so check swings on a `StarterCharacter` copy with the katana welded the way `CombatService.attachKatana` does it (grip at the bottom of RightHand).
- **Re-measuring slash cuts after editing a swing animation:**
  1. Step it on that copy and record `RightHandle.TrailTip` in HumanoidRootPart space.
  2. Find the cut where the tip speed peaks, ignoring the frame-one snap from the stance.
  3. Fit a circle: the axis is the sum of (pᵢ−c)×(pᵢ₊₁−c), then a Kasa fit in the plane. Up is the middle angle of the cut.
  4. Write the result into `Config.SlashFx.Cuts` (2026-11-09).
- **`require` keeps the old module after `UpdateSourceAsync` in Edit mode:** the same session returns the cached table. To test an edit, require a fresh clone parented next to the original (so `script.Parent` still resolves), then destroy it (2026-11-10).
- **`Region3int16` has no `CFrame`:** use `.Min` and `.Max` (2026-11-10).
- **`Lighting.Technology` can't be read from the command bar or the MCP** (capability error). Don't read it in tooling code (2026-11-10).
- **Terrain `ReplaceMaterial` repaints without reshaping:** use it with a 4-stud-aligned `Region3` (`:ExpandToGrid(4)`) for streets, squares and ploughed fields on generated terrain (2026-11-10).
