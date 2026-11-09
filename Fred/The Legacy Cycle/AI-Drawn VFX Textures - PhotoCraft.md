---
tags: [legacy-cycle, research, youtube, vfx, flipbook, photocraft, mcp, slash]
updated: 2026-11-09
---

# AI-Drawn VFX Textures - PhotoCraft

Study of YorusDevs, "How Opus 5.5 drew every visual effect in my Roblox game" (https://www.youtube.com/watch?v=7VQpF5ERE0M, 5:01). These are my own notes, not a transcript. It is a companion to [[Sword Animation Pipeline - Retarget and Mocap]], from the same creator and game.
- Back to [[The Legacy Cycle]]. Related: [[Roblox VFX Techniques]], [[VFX and Animation Training Log]].
- The full how-to lives in the LihFolk vault: `07 Lighting VFX and Audio/AI-Drawn Layered VFX Textures with PhotoCraft - Studied from YouTube.md`.

## The workflow

1. **Connect an editor over MCP.** PhotoCraft (github.com/storytold/photocraft, free, MIT or Apache-2.0) ships an MCP server. The agent clones it, builds it, connects, then draws with layers, paths and filters, and screenshots its own work.
2. **Draw with code, not a generator.** Write the outline as a point list, apply written style rules, tear the edges along the flow, add small rips.
   - Slash rules: asymmetric, a thick head and a thin tail, no flat ends, a clean arc, small voids.
3. **Use four white layers: rim, body, hot core, glow.** The game tints each layer, so one drawing makes every element (fire, ice, poison, void).
4. **Make a 4×4 flipbook** that grows in, lives and breaks apart. Taper both ends on every frame.
5. **Stack the layers on the swing in game**, then play the frames.

## Guards to copy

- Edge check on every texture: no paint within 4 px of the border, or a square shows in game.
- **No red droplet shapes.** They read as blood and risk a Roblox moderation warning. Red is only for glow, flame or smoke.
- The AI has no taste, so a human reviews against written notes and asks for a redraw.

## How it fits our setup

- **PhaseVFX still comes first** for stock effects (standing preference). Use this pipeline when PhaseVFX has nothing that fits, or for signature effects like the katana slash that have to match our animations.
- It improves on our earlier procedural flipbooks (the slash flipbook drill in the Training Log) in three ways: white layered sheets, re-tinting per element, and the 4 px edge check.
- **Pair it with the animation pipeline.** The baked clips store the blade pose per frame, so slash layers can be drawn along the actual blade path.

## Skill

Run **`/draw-vfx`** in Claude Code to make new effect textures this way (`~/.claude/skills/draw-vfx/`).
- It does a RAM preflight, writes the style rules, draws through PhotoCraft, then runs the edge and white gates and builds a review sheet.
- The first passing sheet is the reference slash, `VFX Training/photocraft/slash_v5/`.

## Setup (done 2026-11-09)

- **App:** PhotoCraft v0.22.0, the signed Windows portable build (SHA256 checked) at `C:\Users\USER\Documents\Tools\PhotoCraft\`. The source checkout for the docs is at `Tools\photocraft\`. No Rust build was needed.
- **MCP:** registered at user scope as `photocraft`, running headless (`photocraft-cli.exe mcp`). Read and write are both rooted at `Lihfolk_Vault\VFX Training\photocraft\`, so all output lands there. A new Claude session is needed to pick it up.
- **Commands that work:**
  - `doc_new {width,height}`;
  - `path.set {path:{subpaths:[{closed,knots:[[x,y],…]}]}}`;
  - `layer.newFillLayer.solidColor {color:"#ffffff", path:"work"}`, which makes a white shape layer;
  - `layer.duplicate`, `layer.select`, `layer.rasterize.layer`;
  - `layer.delete {layer:2}`, only after another layer exists;
  - `doc_save {path:"x.png"}`, which flattens but keeps alpha.
- **Preview caveat:** `render_preview` flattens onto white, so white art looks blank. Check saved PNGs over a dark background.
- **Known blocker:** filters (Gaussian blur for the glow layer) fail with "not enough working memory" when free RAM is below about 5% of total. PhotoCraft holds back `total/20` as headroom, and no pref overrides it. Close Chrome tabs, Discord or the Epic launcher first.
- The smoke test `smoke_slash.png` is a white crescent with a thin tail and passes the 4 px edge check.
