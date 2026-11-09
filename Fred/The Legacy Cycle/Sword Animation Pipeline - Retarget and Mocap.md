---
tags: [legacy-cycle, research, youtube, animation, retargeting, mocap, freemocap, unreal, combat]
updated: 2026-11-09
---

# Sword Animation Pipeline - Retarget and Mocap

Study of YorusDevs, "How I made all my Roblox sword animations with Opus 5.5" (https://www.youtube.com/watch?v=X8L-mP92zcA, 6:37). These are my own notes, not a transcript. **We're adopting this as the source-of-motion step for Legacy Cycle combat animations.**
- Back to [[The Legacy Cycle]]. Related: [[VFX and Animation Training Log]], [[Roblox Dev Study - Round 2]].
- The full how-to lives in the LihFolk vault: `08 Animation/03 Authoring/Retargeting Sword Animations and Free Mocap to R15 - Studied from YouTube.md`.

## The workflow

1. **Get the moves.** Two sources:
   - Existing Unreal-mannequin packs from Fab (Free filter; check the license allows non-Unreal use).
   - FreeMoCap from 2–3 phone or webcam videos.
2. **Export them** as per-frame numbers:
   - Unreal: a headless Python script dumps every bone's world position and quaternion per frame to JSON.
   - FreeMoCap: use the MediaPipe detector, which outputs a `.blend`, FBX or BVH skeleton.
3. **Move them onto R15.**
   - From Unreal, 71 bones become 15 parts: fingers and twist bones dropped, spine folded. Limbs copy *direction*, not angle.
   - From mocap, aim each part between its two joint points.
   - Then **put the hands on the hilt first and IK the arms to reach.**
4. **Clean up.**
   - Run a blade-vs-body check every frame and move the hands to clear it. They went from about 1,700 clipping frames to 67.
   - For mocap: hold the last good roll on straight arms, put the lowest foot on the ground, and add the weapon separately.
5. **Ship.** The weapon pose is baked per frame into the same data the server reads, so server hits match the visible blade.

## How it fits what we already built

We already have the hard half. `ServerStorage.AnimTraining.R15Swing` and `VFX Training/2026-11-09/r15/r15_blender.py` do:
- per-frame two-bone IK with hands on the hilt;
- `M.clipping` box-overlap checks every frame;
- limb detachment and the `M.plan` Viterbi pole picker.

What the video adds is a **real source of motion** in place of hand-authored hilt keys. That was the weak point: the user called the FK result "too stiff" (see the Training Log).

**Plan:**
1. Source pelvis, chest and foot trajectories, plus hilt path and blade direction, from a real clip (Unreal JSON or FreeMoCap `.blend`). Feed them into the existing key format instead of hand-written `RUN_KEYS`.
2. Keep our IK, clearance and detach pass as the "clean up" step. It is already measured: the worst contact is 0.23.
3. Copy the server idea. Have `CombatService` sample the blade from the same baked frames (hilt and blade per frame), not from a separate hitbox timing. Today we only have a `Hit` marker.
4. Still apply the five key poses and craft rules on top. Captured motion needs snappier anticipation and a protected hit frame.

**Steps 1–3 are built (2026-11-09) as the `/sword-anim` slash skill** (`~/.claude/skills/sword-anim/`).
- `retarget_r15.py` takes any FBX/BVH (Unreal pack, Mixamo, FreeMoCap) and:
  - places the weapon first, with the hilt measured from the shoulder socket by arm-length scale;
  - runs our toolkit's IK and planner;
  - reports clipping frames;
  - writes R15Swing keys plus `weapon[frame]` CFrames for `CombatService`.
- The self-test round-trips the KatanaM1 clip through a turned, scaled BVH: torso exact, hilt within 0.13 stud, blade within 5°.
- **Blender only: we skip Unreal (decided 2026-11-09).** UE 5.8 crashed with "Out of video memory" while importing a pack. Packs go FBX → Blender → R15 directly.
- **`--unarmed` mode** covers punches and kicks:
  - both hands follow the source;
  - fists are pushed out of the block head (a boxing guard clipped on every frame);
  - the hit is the fastest limb's farthest reach.
- Output is now the **solved per-frame joint poses**, which Studio bakes as-is, with no re-solve.
- Not yet done: the Studio bake.

## Before we start: check these

- Is Unreal Engine installed on this machine? It's needed only for Route A. FreeMoCap and Blender cover Route B without it.
- FreeMoCap is AGPL-3.0. That's fine as a tool; we only ship the animation data it produces.
- Fab licenses: confirm non-Unreal use is allowed for any pack we buy.
- Mocap quality depends on camera frame rate. Film fast sword moves at 60 fps or more; the demo was 6 fps.
