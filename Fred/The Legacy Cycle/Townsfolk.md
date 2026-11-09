---
tags: [legacy-cycle, system, npc, worldbuilding]
updated: 2026-11-09
---

# Townsfolk

Havenbrook's 12 townsfolk walk the town, glance at players, comment as players pass, and talk through reply choices. Their answers come from Roblox's built-in AI, held to each person's character, with written lines as the fallback. Back to [[The Legacy Cycle]]. Decisions are in [[Decisions]] (Townsfolk).

## Who lives here

| Person | Role | Where they walk | Their own topic |
|---|---|---|---|
| Tobin | the Smith (Act 1 outline) | his stall, the shop front, the square | "What do you make of my blade?" |
| Orla Venn | Guildmaster (Act 1 outline) | the quest board, the keep, the square | "What does the guild do?" |
| Mira | Fruit Seller | behind the fruit stalls, the fountain | "Heard any rumours?" |
| Old Wendel | Retired Watchman | the fountain, the square, the board | "Tell me about Ser Caelan." |
| Pip | Town Urchin (a child, 0.72 scale, runs) | round the fountain | "Do you want to be a hero?" |
| Sister Aveline | Keeper of the Lamps | the keep, the fountain, a doorway | "Why tend the lamps?" |
| Garrick | Gate Watchman | the gate, the gate court | "Is the road safe?" |
| Hessa | Farmer | the market, her door | "Trouble at the farms?" |
| Doran | Caravan Clerk (his brother drove the missing caravan) | the board, the gate court | "Tell me about your brother." |
| Lyle | Wandering Bard | the fountain, the square | "Sing me something." |
| Bram | Porter | the gate, the market, doorways | "Seen anything strange?" |
| Edda | Baker | doorways, the market, the fountain | "Got anything to eat?" |

Everyone except Orla and Tobin is a proposal, and so are all the personas and lines. Orla and Tobin are walking townsfolk for now; they give no quests.

## How it works

- **The roster** is in `ReplicatedStorage.Townsfolk`: name, title, persona, look, walk speed, stops, greetings, remarks, "who" lines and one topic each. It also holds the lore the AI may use, `pick` (the most specific line that fits) and `choices`, with self-checks.
- **Context** a line can depend on: the Ember, met before (this server), blade drawn, the active quest, finished quests. For example, Doran's lines change once the caravan quest is done, and Garrick tells a player with a drawn blade to put it away.
- **The server** (`ServerScriptService.Townsfolk`):
  - Bodies: each person is spawned from the hero's own body (StarterCharacter), dressed with `Appearance.apply`, and given a nameplate and an E "Talk" prompt.
  - Movement: they walk between their stops in `Havenbrook.Townsfolk.Stops` on pathfinding routes and linger 6–14 s at each. They pass through each other (collision group `Townsfolk`), and players still bump them.
  - Animations: Roblox's stock walk, idle and wave, the same ones the hero's Animate script uses.
  - Conversations: `TownsfolkAction`, a RemoteFunction with `open`, `ask` and `bye`. The server checks the person, the distance (14 studs) and that the topic was offered.
  - Talking: the person stops, turns to the player, and waves the first time they meet. A conversation ends when the player walks 18 studs away, goes quiet for 60 s, leaves or respawns.
- **The AI** (`ServerScriptService.Townsfolk.Mind`):
  - Setup: one `TextGenerator` per person, with their persona, the lore and the rules (one or two sentences, stay in the world, no invented places or events, no quests, items, prices or rewards).
  - The prompt: the moment's facts (level, Ember, blade, met before, quest, time of day) and the written line as the gist.
  - Cleaning: replies are tidied and capped at 220 characters. An out-of-world reply is dropped.
  - Fallback: any error, a timeout over 6 s, or more than 80 requests a minute → the written line.
  - The `UseAI` attribute on the server script switches the AI off.
- **The client:**
  - `DialogueClient` shows the reply choices (click, 1–4, or the gamepad), a pulsing ellipsis while an answer is coming, and the AI notice.
  - `TownsfolkBarks` shows a remark when you come within 10 studs. The chance is 45%, each person waits 35 s between remarks, and remarks are at least 5 s apart.
  - Remarks and reactions use our own bubble, `TownsfolkBarks.Bubble`, not Roblox's chat bubble: italic text with no background, over a thin gold rule, animated in 5 beats.
- **Reactions** (2026-11-09): bumping into someone, or hitting them with a swing, gets a shove, a turn toward you and a line from `Townsfolk.Reactions` (two "bump" and two "struck" lines each). The server watches bumps every 0.1 s (closing speed over 1.5 studs/s within 3.4 studs). Hits come from `CombatCore.onBystanderHit`. Cooldowns are 4 s for bumps and 1.2 s for hits. The `TownsfolkReact` remote sends the line to players within 40 studs.
  - `Locomotion` gives townsfolk the same lean, foot planting and head turn as combatants. They look at the player they're talking to, otherwise at the nearest player within 12 studs.

## Roblox's AI rules (2026-11-09)

From Roblox's generative-AI documentation:
- **Disclosure:** players must see that the conversation is with AI, at the start and throughout. The dialogue panel shows "AI-powered conversation, not a human. It may make mistakes." for the whole townsfolk conversation. Roblox also prefixes its own notice to the first reply; that is stripped, since the panel already shows it.
- **Moderation:** Roblox moderates TextGenerator input and output, and holds its models to All Ages. Replies go only to the player talking.
- **Keep it non-"extended":** "extended AI interactions" (unlimited continuous AI chat, or AI memory across sessions) require the **Restricted (18+)** maturity label. So:
  - AI memory (the ContextToken) lasts one conversation and is never saved.
  - Each person gives a player at most 6 AI answers per 15 minutes, then written lines.
  - Players can only pick choices, never type.
- **Before publishing:** answer the Content Maturity questionnaire's AI questions for the AI talk. It is not "extended" as built.

## Limits and next steps

- "Met before" is remembered per server only. Saving it would need a PlayerData field. Saving the AI's context across sessions would make it "extended", so don't.
- There is no day and night cycle yet (ClockTime stays at 17.5). The facts already say morning, afternoon, evening or night, so routines could follow a cycle later.
- Townsfolk react to being bumped or hit, but not yet to fights nearby (no fleeing or cowering).
- Captain Aldric is unchanged: still a scripted conversation that gives the caravan quest.
- **Dev space crowd (for testing):** while `workspace.Dev.Townsfolk.Stops` exists, the server spawns a second copy of all 12 there, each wandering between any of its 9 stops (Dev1–Dev9, on open baseplate north of the spawn, out of the Squire's leash). They talk, remark and use the AI like the town's. Delete that folder, or the Dev space, before release.
- 3 houses (H2, H5, H7) had no clear ground by their doors, so they have no stops.
