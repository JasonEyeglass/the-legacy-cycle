---
tags: [legacy-cycle, research, youtube, world-building, blender, mcp, procedural, breakables]
updated: 2026-11-09
---

# World Building Pipeline - Blender Scripts

Study of YorusDevs, "How Opus 5.5 built my entire game map in Blender" (https://www.youtube.com/watch?v=Ba0kl4rTOLo, 5:56). These are my own notes, not a transcript. It's the third video in the series, after [[Sword Animation Pipeline - Retarget and Mocap]] and [[AI-Drawn VFX Textures - PhotoCraft]]. **We're adopting it for world building and model making.**
- Back to [[The Legacy Cycle]].
- The full how-to lives in the LihFolk vault: `45 Procedural Blender Assets/Scripted World Building in Blender via MCP - Studied from YouTube.md`.

## The workflow

1. **Connect.** Use Blender's official MCP (Blender 5.1+). Drag the add-on in twice, turn on Allow Online Access, and wait for "server is running". It runs agent code with no guards, so save first.
2. **Talk, for one model.** The agent builds, screenshots the viewport, critiques and fixes. Their sword-in-stone island took 760 triangles and under 3 minutes.
3. **Script, for the game.** Have the agent write a headless build script (`blender --background --factory-startup --python build_x.py`) with every size a named constant at the top.
   - Their arena is 1,274 lines, builds in 8 s and has about 70k triangles. Changing the tiers from 6 to 12 re-flowed everything.
4. **Kits, for zones.** The script builds the pieces and the game's code places them. That gave 8 zones and 1,037 meshes, plus 24 garden styles defined as colour-and-shape lists.
5. **Breakables.** Pre-cut each one 4 ways (horizontal, two diagonals, vertical) with filled faces, giving 8 shards. In game, pick the cut that matches the swing plane. Floors use 8 pre-cut wedges.
6. **Checks.** Inside-out faces (Roblox hides back faces), flicker (offset coplanar faces by 0.05 stud), and a rebuild when it "lacks detail". Every fix becomes a rule in the script.

## How it fits what we have

- **The Blender MCP is already connected here.** The `Blender` tools (execute code, viewport screenshots, scene summaries, API docs) are what the video uses. Before relying on it, check that the Blender version is 5.1+ and that the add-on is the official Blender Lab one.
- **Our scripted Blender tooling already matches step 3:** LihFolk `45 Procedural Blender Assets/scripts`, the `roblox-blender-modeling` skill, and the r15 builder. What's new is turning **whole zones into kit scripts** and **directional pre-cut breakables**.
- **Breakables pair with the combat pipeline.** The retargeted sword clips store the blade pose per frame, so the server already knows the swing plane at the `Hit` frame. Use it to choose the pre-cut.
- **Scope:** apply this to the Act 1 slice zones first, per the slice checkpoint, rather than all 8 zones at once.

## To do when we start

- [ ] Confirm the Blender version and the MCP add-on source.
- [x] Turn one Act 1 zone into a `build_<zone>_kit.py` with top-level constants and a Roblox export (2026-11-10). `Legacy Cycle World/build_havenbrook_kit.py` builds the Havenbrook and Nature kits, and `ServerStorage.WorldPlacer` places them on the `WorldGen` continent.
- [ ] Import `Havenbrook.glb` and `Nature.glb` into `ServerStorage.WorldKit`, then rerun `WorldPlacer.place()`. The placer reports whether the kit came in turned 180°.
- [x] `precut.py` written and tested (2026-11-09). It's in LihFolk `45 Procedural Blender Assets/scripts/`.
  - It makes 4 exact-Boolean cuts with filled faces in an inner colour, exporting a GLB of the intact model plus 8 shards and a JSON of cut normals.
  - All gates pass on a demo post and on the Grow an Ember brazier.
- [ ] Import one pre-cut GLB in Studio and check the 180° glTF turn against `normal_roblox`.
- [ ] Write the runtime picker: the swing-plane normal comes from the blade at the `Hit` frame, then take the max |dot| cut, swap in its shards and push them apart.
- [x] Pre-export checks built: `check_mesh.py` covers inside-out parts, coplanar overlaps and the triangle budget (2026-11-09).
- [x] **`/build-world` slash skill** created (`~/.claude/skills/build-world/`), with a zone-kit template (forest/snow styles), the mesh gate and the precut usage. Run `/build-world` to start any model or zone.
