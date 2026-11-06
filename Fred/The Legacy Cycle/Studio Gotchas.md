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
