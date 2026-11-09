---
tags: [legacy-cycle, home]
updated: 2026-11-06
---

# The Legacy Cycle

A dark-fantasy open-world Roblox RPG with R15 characters and katana combat. It is built phone first, with at most 8 on-screen buttons, and it is rated Mild.

- **Roblox place:** 136951107164479
- **Team design repo:** `JasonEyeglass/the-legacy-cycle` (private). The Obsidian vault there is under `THE LEGACY CYCLE/`, and the settled calls are in `3 ✅ Tracking/Decisions.md`. The GDD v0.2 .docx is at the root.
- **These notes:** written from the Claude Code build sessions, and mirrored to the repo under `Fred/`.

## Map

- [[Decisions]]: every settled design call, with its date.
- [[Systems Map]]: every Studio module, with what it does and where it lives.
- Player-facing systems:
  - [[Join Flow]]: main menu, then the loading pan, then forge your hero or continue.
  - [[Character Creator]]: hero presets, saved looks, and the Shrine of Embers.
  - [[Settings]]: Music and Sound effects sliders, plus Camera shake and Low specs.
  - [[Floating Menu]]: the in-world M menu (Inventory, Stats, Embers, Back to Main Menu).
  - [[Shrine VFX]]: the new Shrine of Embers mesh, with native and Blender-flipbook fire side by side.
  - [[Townsfolk]]: 12 Havenbrook townsfolk who walk, glance, remark and talk through reply choices answered by Roblox's AI.
- [[UI Kit (Ornament)]]: the fantasy UI helpers and the gotchas they work around.
- [[Studio Gotchas]]: Roblox Studio and Studio MCP lessons from building this.
- [[Roblox VFX Techniques]]: VFX and animation recipes studied from 13 YouTube tutorials, and the AI video-watching repos.
- Dev log: [[2026-11-06]].

## Key locations

| What | Where |
|---|---|
| SpawnLocation (Dev test area) | (-217, -5, 3039) |
| Havenbrook town | roughly (0, ?, 230), about 2,800 studs from spawn |
| Loading pan shots | `Workspace.Cinematics.LoadingPan` (Persistent model) |
| Claude Design mockups | https://claude.ai/artifact/3uDM3AaNB91bq9dboo7nb8 |
| Adventuring roadmap (Claude Docs) | https://claude.ai/artifact/LoWjDdpb5GPxDV5oUorcuT |
| Stat points spec | https://claude.ai/artifact/DE3Npf2uYfJyrjr1rXR7sY |
