---
created: 2026-11-04
owner: JasonEyeglass
tags: [plan, design, import, proposal]
status: proposal
source: Claude doc https://claude.ai/artifact/RYcnM1DfjWvGdB8aXBsJBW
---

# Roblox Action MMO — Game Plan v0.1

Part of [[Chat Import Hub - Roblox Action MMO]]. Research behind it: [[Research - What Made Roblox Hits and MMOs Work]]. Compare with [[Project Plan Overview]].

## Summary

We build an original anime-style action MMO on Roblox: hack-and-slash combat that looks great in short clips, a story players don't want to skip, and fair, cosmetics-only sales. We launch small and polished (one zone, 30–60 minutes of story, one boss, one raid, a drop-in arena), then release a new story chapter about every 6 weeks.

- **Combat first.** If a 20-second fight isn't fun, nothing else matters, so combat is the first thing we prototype.
- **The story is planned like a TV season.** The whole arc and its big mysteries are written before launch and revealed one chapter per update.
- **Mobile and PC from day one**, designed around mobile limits.
- **No pay-to-win, ever.** Money buys looks, never power.

The choices that need a team decision are marked **Decide** in each section. Questions for the team are at the end.

## Team: one owner per area, plus an integrator

Give each of the four people one area they own and decide on, even though everyone codes with AI. The LihFolk Vault's note on small teams says that at 3 to 4 people you need an **integrator**: one person who merges everyone's work into the game and runs each release. Without one, the project fills with folders nobody owns.

| Person | Owns | Also does |
| --- | --- | --- |
| A — Lead and combat (integrator) | Combat, abilities, hit detection, server code, saves, release | Final say on game design |
| B — World and story | Story bible, quests, dialogue, zone layout and building, enemy placement | Writes the play-test checklist for each chapter |
| C — Characters and motion | Models in Blender, rigs, animation, VFX for abilities and ultimates | Art-style guardian |
| D — UI, audio and community | Mobile-first UI, music and sound, shop, Discord, TikTok/Shorts clips, analytics | Play-test lead |

**How much time you really have.** Most hours don't turn into finished work. The vault's rule of thumb: weekly capacity = hours per person × a focus factor, which is 0.4–0.5 for evening or part-time work. Four people at 15 hours a week is about 27 productive hours. Plan the timeline against that number, not against 60.

**Rules for building with AI**, taken from the vault's post-mortem of an AI-built Roblox melee game:

- **The server decides everything.** Damage, cooldowns, drops and purchases are checked on the server, because exploiters control the client.
- **Use real version control** (Rojo + git), not backup copies inside the game. In the post-mortem, backups inside the place grew to about 1 million instances and slowed Studio to 15 fps.
- **Never use undo in a shared place.** It also rolls back teammates' work.
- **Ownership map.** Each folder has an owner, and nobody moves work they didn't make without asking.
- **Schedule play-tests.** AI can check numbers in Edit mode, but only a human can tell whether combat feels good. In the post-mortem, enemies needed 40–100+ hits to kill and nobody noticed until someone played.
- **Give the AI a visual reference** (a video or picture), not adjectives. Ask it to report numbers back, like "damage per second before and after."
- **Own the game through a Roblox group**, not a personal account, so the game stays if someone leaves.

You already have Roblox Studio and Blender connected to Claude on your computer, so models and scripts can move between them directly.

## Setting and story

My recommendation: an original anime-style world with dark-fantasy themes but a hopeful tone (option 1 below). Anime style draws the biggest audience right now. Themes like loss and sacrifice are fine for kids as long as the violence stays stylised (see Tone).

| Option | The world in one line | Good for | Risk |
| --- | --- | --- | --- |
| **1. The Shattered Sky (recommended)** | A cataclysm broke the world into floating islands above a sea of fog. People fight with the spirits of what was lost. | Each island is one zone and one chapter, so new islands are the natural update unit. Bright enough for kids, mysterious enough for teens. | Floating islands are common in fantasy, so the twist has to come from the story. |
| 2. Spirit city | A modern anime city where spirits are born from human emotions | Strong teen appeal, easy to make cool | Too close to Jujutsu Kaisen; we would look like a copy |
| 3. Last lantern | A dark-fantasy world swallowed by a living fog, with one surviving city | The strongest mood | Hardest to make kid-friendly, and the art is expensive |

**Story seed for option 1** (a draft to react to, not final):

> One hundred years ago, the Sundering broke the sky. Everything that fell into the fog was forgotten, even by the people who loved it. You wake on a small island with no memory, and a spirit (an Echo) chooses you. Echoes are fragments of what the world forgot. Each one you bond with gives you power and shows you a memory. The big mystery of the saga: *who caused the Sundering, and why can you, alone, remember what others forget?*

Why this seed works for an MMO:

- **Story and power are the same system.** Every Echo you collect reveals a memory, so collecting power also tells the story (see Power system).
- **Islands are chapters.** Each update raises a new island out of the fog.
- **It comes with a built-in mystery:** the player's missing memory. We can pay it off years later.
- **It's hopeful.** The goal is to remember and restore, not just survive.

**Decide:** pick option 1, 2 or 3, or bring your own. The name "Shattered Sky" is a placeholder. (Note: the existing [[Story Foundation]] is a fourth option; see the comparison in [[Chat Import Hub - Roblox Action MMO]].)

## Power system: Echoes plus weapon styles

My recommendation: two layers. A **weapon style** gives your basic combo. An **Echo** gives your special moves and your ultimate. Mixing the two creates builds. This takes the parts that made the popular systems work without copying any of them.

| Popular system | Game | Why players love it | What we take |
| --- | --- | --- | --- |
| Devil-fruit-style fruits | [Blox Fruits](https://naavik.co/digest/blox-fruits-lessons/) | Rare finds, one at a time, trading | Rarity, and a real choice of one active power |
| Character move sets | [Jujutsu Shenanigans](https://rowatcher.com/news/why-anime-battlegrounds-are-taking-over-roblox-in-2026) | 4–6 clear moves and a cinematic ultimate that's easy to clip | The 4-moves-plus-ultimate layout |
| Clans and breathing styles | Demon Slayer-inspired games | A personal identity | The Echo as your identity |
| Awakenings and mastery | Blox Fruits | Long-term goals for veterans | Echo mastery that unlocks an awakened form |

**How Echoes work:**

1. You earn Echoes in the world: from bosses, story quests, hidden places and events. They are never sold for Robux.
2. You equip one Echo at a time. Each has 4 moves plus 1 ultimate, and each favors a role: damage, control, support or tank.
3. Using an Echo raises its **mastery**. Mastery unlocks its moves, then its **memory** (a short story scene), then its awakened form.
4. Each weapon style (sword, gauntlets, spear, scythe at launch) has its own M1 combo, dash and parry. Any Echo works with any weapon style.

**Why this is the "fresh" mechanic:** in other games, power and story are separate. Here, every new power reveals a piece of the mystery. Players chase Echoes for strength and get the lore without reading walls of text.

Start small at launch: 3 weapon styles and 6 Echoes. Each update adds 1–2 Echoes tied to the new island's story.

**Decide:** do Echoes also roll randomly (like fruit spins)? Random rolls are exciting, but selling rolls for Robux brings odds-disclosure rules, and some regions block paid random items. I suggest rolls are free and earned only.

## Tone and age rating: aim for "Mild"

Yes, the game can be all ages, and it should be. Roblox sorts games by a content-maturity label, and the label decides who can see your game on Home and Charts. The LihFolk Vault's compliance note (checked against Roblox docs, October 2026) lists these labels:

| Label | Who sees it by default |
| --- | --- |
| Minimal / Mild | Roblox Kids (5–8), Roblox Select (9–15) and up |
| Moderate | Roblox Select (9–15) and 16+ |
| Content May Vary | Age-checked 16+ only |
| Restricted | Age-verified 18+ only |

A sword-combat game with stylised violence usually rates Mild or Moderate. Strong violence or heavy realistic blood moves a game to 18+ only. Aiming for **Mild** gives us the largest audience.

**How to be "dark but fair" and still Mild:**

- **No blood.** Enemies break into light, ash or fog when defeated.
- **Dark themes are fine.** Loss, sacrifice and forgotten people work for every age; think Pokémon or Avatar. FFXIV's story is dark yet [stays optimistic](https://gamespot.com/articles/ffxiv-53-inspires-hope-closing-one-of-final-fantas/1100-6481156/).
- **Cool, not cute.** A sharp anime style keeps teens; a babyish look loses them.

**What turns each age group away, and the fix:**

| Group | Turned off by | Fix |
| --- | --- | --- |
| Kids (9–12) | Walls of text, getting stuck, scary gore | Short scenes with voice or pictures, a clear next-objective marker, no gore |
| Teens (13–17) | Babyish art, no challenge, story that interrupts play | Cool art, hard bosses and raids, scenes under 60 seconds |
| Story fans | Being forced to skip | Skippable scenes plus a Memory Journal to rewatch anything |

After every update that changes content, resubmit the maturity questionnaire. Roblox requires it.

## Scope: PvE and PvP, built on one combat system

We can launch with both PvE and PvP, at the quality we want, because both use the same combat code. The arena reuses the same Echoes, weapon styles and moves as the story, so PvP is mostly a matchmaking and balancing job, not a second game. What would be too ambitious is a big world plus open-world PvP plus trading plus guilds, all at launch.

| Launch (v1.0) | First updates (v1.1–v1.3) | Later (Season 2+) |
| --- | --- | --- |
| Island 1: 30–60 min of story | One new island and chapter per update | Guilds |
| 3 weapon styles, 6 Echoes | 1–2 new Echoes per update | Trading (with paid-item rules) |
| 1 mini-boss, 1 boss | Echo awakenings | Ranked PvP seasons |
| 1 raid (3–4 players) | A hard mode for the raid | World bosses and server events |
| Arena: 1v1 and free-for-all | Team arena 2v2 or 3v3 | Housing or a home island |
| Shop, battle pass, saving, settings | Memory Journal | Crafting depth |

**Rules that keep scope safe**

- **No open-world PvP.** PvP happens only in the arena or with a PvP flag that players switch on. Being attacked by strangers drives away kids and story players.
- **Bosses must not block progress.** The vault's combat notes report a game that lost 82% of new players at the first boss. Use short, telegraphed boss fights, show a "Recommended power" number, and let players walk away and retry for free.
- **Cut by layer, not by feature.** If we fall behind, ship fewer Echoes or fewer animations rather than dropping the raid.
- **Every new idea must name what it replaces**, or add its hours to the plan in the open.

## Platforms: build for phone and PC from day one

Build for both at the same time, but design every feature for the phone first. About [80% of Roblox players are on mobile, 17% on desktop and 3% on console](https://www.pocketgamer.biz/80-of-roblox-users-are-on-mobile-contributing-46-of-robux-revenue). Adding phone support later costs far more than building it in from the start: the post-mortem in the vault had to rebuild about two dozen screens to make them work on phones.

Supporting phones doesn't limit the plan. It sets some design rules:

| Phone limit | Design rule |
| --- | --- |
| A thumb, not a mouse | M1 combo, dash, block/parry, 4 Echo moves and 1 ultimate: no more than 8 buttons. Soft lock-on instead of precise aiming. |
| Small screen | Large tap targets and minimum text sizes. Test every screen at two phone resolutions. |
| Weak and hot phones | A VFX budget with quality tiers. Reuse effects instead of creating new objects on every hit. Few full-screen effects. |
| Download size | Keep shared assets small. In the post-mortem, cutting the content every player downloads from 392k to about 70k objects was a big part of the fix. |
| Short sessions | Every play session has a 10–20 minute option: one quest, one arena match, or one raid attempt. |

PC players get keybinds, and controller support comes later. The PC version is the phone design plus hotkeys.

## Monetization: battle pass plus cosmetics, never power

We sell looks and support, never strength. The battle pass is the main earner, and each pass season lasts one story chapter, so money and story share one schedule. Compare with [[Economy and Monetization]].

**What we sell**

| Product | Type | Examples |
| --- | --- | --- |
| Battle pass (premium track) | Developer product, recorded per season | Chapter-themed skins, auras, titles, emotes, a final cosmetic |
| Cosmetic shop | Developer products and game passes | Weapon skins, Echo skins (recolors and new ultimate effects), kill effects, emotes, name colors |
| Weekly rotating shop | Developer products | "This week's item": it rotates back later, so don't call it "limited" |
| Private servers | Roblox private servers | For friend groups and content creators |
| Supporter pack | Game pass | A badge, a title and a cosmetic set. No power. |

**Battle pass design** (from Roblox's own season-pass guidance, as summarized in the LihFolk Vault):

- **One pass per chapter.** About 5 weeks of pass, then about 1 week of rest before the next chapter. Roblox suggests roughly one-month seasons with at least a week of rest, and this matches a 6-week story cycle.
- **Start with about 10 tiers.** Every player is on the free track automatically, and the free track ends with its own small final reward.
- **Daily missions** use at least 3 categories (story, combat, arena) and never require spending Robux.
- **Completable.** Set total XP to about 80–85% of what a 5-days-a-week player earns, so sick days don't cost the reward. Double XP in the last week.
- **No penalty for buying late.** A player who buys premium mid-season gets every premium reward for the tiers already reached.
- **Fixed rewards only.** If the premium track contains no random crates, the odds-disclosure rules don't apply to it.

**Fairness rules (the "never break the game" list)**

1. No Echoes, weapon styles, stats, XP boosts or skips for Robux. In PvP, anything that adds power is pay-to-win.
2. No paid loot boxes. Paid random items need odds shown before purchase, and some regions block them.
3. Honest timers. Call an item "exclusive" only if it truly never comes back. Otherwise say "returns later."
4. No fake countdowns or guilt-trip notifications.

For reference: developers cash out at [$0.0038 per Robux](https://rowatcher.com/news/roblox-monetization-strategies-that-actually-work-in-2026) ($0.0054 for spending by US players 18+).

**Decide:** the premium pass price. Pick a test price before launch and adjust it using real sales data.

## Story that grows with every patch: the formula

We write the ending first, then release the story one chapter every 6 weeks, and each chapter answers one question and asks a new one. This is the method behind the live games with the best stories:

- **Genshin Impact** ships a new version [about every six weeks](https://www.pockettactics.com/genshin-impact/update), each with new main-story acts.
- **Destiny 2** planted a mystery in 2018 (the 15th Wish) and [paid it off years later](https://www.gamespot.com/articles/how-destiny-2-is-ditching-seasons-for-less-predictable-episodes/1100-6520642/). Its team aimed for [6–8 weeks of story per season, each with a clear beginning, middle and end that feeds a bigger arc](https://gamesradar.com/how-bungie-made-savathun-destiny-2s-best-villain-yet-and-turned-seasons-into-gripping-tv-shows).
- **Destiny's warning:** when every season followed the same template, players called it [predictable](https://www.gamespot.com/articles/how-destiny-2-is-ditching-seasons-for-less-predictable-episodes/1100-6520642/), and they disliked [weekly drip-fed story](https://thegamepost.com/bungie-destiny-2-seasonal-story-plans-edge-of-fate-renegades/). Release each chapter whole, and change the shape now and then.

### Three layers of planning

| Layer | Length | What we plan before it starts |
| --- | --- | --- |
| Saga | 1–2 years | The ending and the secret behind the Sundering. Written before launch; never changes. |
| Season | 3 chapters (about 4–5 months) | One villain or conflict, one big reveal at the finale |
| Chapter | 6 weeks | One island, 1–2 Echoes, one boss, one battle pass |

### The chapter template

1. **Hook:** open with a question or a threat in the first minute, never a history lesson.
2. **New island:** a title card, then a landmark that shows what happened there.
3. **A person to care about:** one local character with a want. This is FFXIV's lesson: [a personal connection](https://novacrystallis.com/2020/08/one-year-later-the-story-of-final-fantasy-xiv-shadowbringers/) shown, not told.
4. **A new Echo** whose memory reveals part of the island's secret.
5. **The boss** as the climax.
6. **Payoff and cliffhanger:** answer one old question and ask one new one.

### Delivery rules

These come from the narrative notes in the LihFolk Vault:

- **Write for three kinds of reader.** *Surface* players get mood and landmarks with almost no text. *Curious* players get a line per item and optional NPC talk. *Obsessed* players get secrets, hidden rooms and contradictions. The obsessed group is small, but they make the YouTube theory videos and the wikis.
- **Keep text short.** Codex entries run 30–60 words, and an NPC's first conversation stays under 50 words.
- **Keep a story bible.** It holds the timeline, places, characters, a glossary and an **open-questions table** with each mystery's true answer and the update that reveals it. Nothing ships without its bible entry.
- **Never break a promise in quest text.** If a line says "come back tomorrow," something must actually happen tomorrow.

### Mystery ledger (example)

| Mystery | Planted | Paid off |
| --- | --- | --- |
| Why can the player remember what others forget? | Ch. 1, first scene | Saga finale |
| Who is the masked figure at every Echo shrine? | Ch. 1 boss | Season 1 finale (Ch. 3) |
| What sank island 4? | Ch. 2, a contradiction between two NPCs | Ch. 4 |

## Roadmap and timeline

Plan for a first playable combat prototype in about 2 months and a public launch in about 8 months. Each phase ends at a gate, a test the game has to pass before the next phase starts. If a gate fails, fix that phase first; the gates matter more than the dates.

| Phase | Weeks / dates | What gets built | Gate to pass |
| --- | --- | --- | --- |
| 1. Pre-production | Weeks 1–3 · Nov 2026 | Story bible, art-style sheet, Echoes and weapons on paper, Roblox group, Rojo and git | **Pitch locked:** setting, power system and art style agreed by all four |
| 2. Combat prototype | Weeks 4–9 · Dec 2026–Jan 2027 | Grey-box arena: 1 weapon style, 2 Echoes, 1 enemy type; plays well on a phone | **Fun test:** outside testers ask for another round |
| 3. Vertical slice | Weeks 10–17 · Jan–Mar 2027 | First 10 minutes of story, starter area, 1 mini-boss, saving and core UI | **First 10 minutes:** most testers finish it without help |
| 4. Launch build | Weeks 18–28 · Mar–May 2027 | Island 1 complete: boss, raid, arena, 3 weapon styles, 6 Echoes, shop and battle pass | **Content complete:** no blocking bugs; runs smoothly on a mid-range phone |
| 5. Closed beta | Weeks 29–32 · Jun 2027 | Discord testers, bug fixing, maturity questionnaire, thumbnails, trailer clips | **Launch:** target late June 2027 |
| 6. Live: Season 1 | Every 6 weeks after launch | Chapter 2 around Aug 2027, Chapter 3 and season finale around Sep 2027, then Season 2 | — |

The dates assume about 15 hours a week per person, starting Nov 9, 2026. At 8 hours a week, roughly double each phase. We'll update this once everyone's real hours are known (see Open questions). These phases map onto the GDD milestones A–E in [[Project Plan Overview]].

## Open questions for the team

- [ ] How many hours per week can each of the 4 people give? The timeline depends on this.
- [ ] Who takes which role (A combat/lead, B world/story, C models/animation/VFX, D UI/audio/community)? Does anyone already have Blender or animation experience?
- [ ] Setting: Shattered Sky, spirit city, last lantern, the Legacy Cycle story, or your own idea?
- [ ] Power system: are Echoes plus weapon styles the direction? Should Echoes also roll randomly (earned rolls only)?
- [ ] Combat feel: fast and flashy like Jujutsu Shenanigans, or heavier and deliberate like Deepwoken?
- [ ] Is a Roblox group set up to own the game?
- [ ] Is there a small budget for Roblox ads or sponsored placement at launch, even US$50–200?
- [ ] Is there a hard date you want to hit, like a school break or a holiday?

## Sources

- LihFolk Vault V1: Scoping and Roles for Small Teams; Post-Mortem – Building a Melee Action Game; Working with AI Assistants in Studio; Genre Playbook – Combat and RPG; Roblox Policies, Ratings and Compliance; Battle Pass and Season Design; Limited-Time Offers; Lore Delivery Without Walls of Text; Quest Writing and Hooks; Juice on a Budget – Mobile
- [Naavik: Blox Fruits lessons](https://naavik.co/digest/blox-fruits-lessons/)
- [RoWatcher: Why anime battlegrounds are taking over Roblox](https://rowatcher.com/news/why-anime-battlegrounds-are-taking-over-roblox-in-2026)
- [RoWatcher: Roblox monetization strategies 2026](https://rowatcher.com/news/roblox-monetization-strategies-that-actually-work-in-2026)
- [PocketGamer.biz: 80% of Roblox users are on mobile](https://www.pocketgamer.biz/80-of-roblox-users-are-on-mobile-contributing-46-of-robux-revenue)
- [Pocket Tactics: Genshin Impact update cadence](https://www.pockettactics.com/genshin-impact/update)
- [GamesRadar: How Bungie turned Destiny 2 seasons into TV shows](https://gamesradar.com/how-bungie-made-savathun-destiny-2s-best-villain-yet-and-turned-seasons-into-gripping-tv-shows)
- [GameSpot: Destiny 2 moves from seasons to episodes](https://www.gamespot.com/articles/how-destiny-2-is-ditching-seasons-for-less-predictable-episodes/1100-6520642/)
- [The Game Post: Bungie on dropping the weekly drip feed](https://thegamepost.com/bungie-destiny-2-seasonal-story-plans-edge-of-fate-renegades/)
- [Nova Crystallis: The story of FFXIV Shadowbringers](https://novacrystallis.com/2020/08/one-year-later-the-story-of-final-fantasy-xiv-shadowbringers/)
- [GameSpot: FFXIV's hopeful tone](https://gamespot.com/articles/ffxiv-53-inspires-hope-closing-one-of-final-fantas/1100-6481156/)
