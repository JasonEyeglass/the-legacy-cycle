---
created: 2026-11-03
owner: JasonEyeglass
tags: [story, script, prologue, draft]
status: draft
---

# Prologue Script: The Last Ember (draft v0.1)

The first 5 minutes of [[Home|The Legacy Cycle]], as set out in [[Story Foundation]]. It teaches the controls, sets up the mystery of the Cycle, and introduces the Ember that the [[Legacy System Spec]] is built on.

**The twist:** the player plays **Ser Caelan**, champion of the last Cycle. At the end, Caelan refuses to pass on his Ember and the Void takes him. He becomes **the Fallen Knight**, the Havenbrook boss the player fights later in Act 1. The player's first character is the first boss they will face.

## At a glance
| | |
|---|---|
| Length | About 5 minutes (target, not measured) |
| Players | Solo. Every new player sees it once. |
| Teaches | Moving, basic attack, heavy attack, dodge, parry, the Ember skill (the GDD's "one magic ability") |
| Ends with | The player waking up in present-day Havenbrook with the first quest |
| Can it be skipped? | Not the first time. Skippable for every later character in the bloodline (see "Heirs" below). |

## Characters
- **Ser Caelan:** the playable champion. Brave, proud, worn out. His flaw is that he cannot let go.
- **The Keeper:** an ageless spirit bound to the Shrine of Embers. Calm and sad. She has watched every Cycle. The player meets her again in present-day Havenbrook, where she runs the retirement shrine.
- **The Void:** never shown clearly. A voice and a darkness. The Void Sovereign stays hidden until the endgame.
- **Hollowed soldiers / the Brute:** Caelan's own soldiers, already taken by the Void.

Writing rule for all lines: short enough to read on a phone in 3 seconds (about 80 characters). Every line can be tapped through.

---

## Scene 0: Cold open (10 s)
**Screen:** black. Embers drift upward. Text fades in line by line, with a quiet voice reading it.

> Every hundred years, the Void wakes.
> Every hundred years, a hero rises to stop it.
> Every hundred years, the hero falls.
> This is the Cycle.

**Title card:** THE LEGACY CYCLE. *Havenbrook, one hundred years ago.*

## Scene 1: Havenbrook burns (about 45 s)
**Teaches:** movement and camera.
**Setting:** old Havenbrook at night. The same streets players will walk later, but burning and broken. Ash falls. The guild hall is on fire.

**Gameplay:** Caelan runs from the town square to the north gate. Burning debris blocks side streets, so the path is linear. Villagers flee past him.

| Who | Line |
|---|---|
| Villager (running) | "Ser Caelan! The north gate, they're breaking through!" |
| Caelan | "Get to the shrine. All of you. Go!" |
| The Keeper (voice, faint) | "Caelan. Your Ember is burning too bright." |
| Caelan | "Then it'll burn long enough." |

**Prompt:** *Move* (WASD / left stick / touch joystick). *Look around* (mouse / right stick / drag).

## Scene 2: The gate (about 90 s)
**Teaches:** basic attack, heavy attack, dodge.
**Setting:** the north gate, half broken. Three hollowed soldiers come through, one at a time, then two together.

| Beat | Gameplay | Prompt |
|---|---|---|
| 1 | One hollowed soldier, slow attacks | *Attack* (click / RT / tap) |
| 2 | Soldier with a shield. Basic attacks bounce off. | *Heavy attack* (hold attack) breaks the guard |
| 3 | Two soldiers. One winds up a big, clearly telegraphed swing. | *Dodge* (Q / B / swipe button) |

| Who | Line |
|---|---|
| Caelan (seeing the first soldier's face) | "Tomas? ...No. You're not him anymore." |
| Hollowed soldier | *(whispering, overlapping)* "Let go... let go..." |
| Caelan (after the fight) | "I'm sorry. I should have been faster." |

**Rule:** Caelan cannot die here. At low HP he recovers slowly, so a new player can't fail the tutorial.

## Scene 3: The Brute (about 60 s)
**Teaches:** parry.
**Setting:** outside the gate, the road to the old Shrine of Embers. A huge Void Brute blocks the road.

**Gameplay:** the Brute's attacks have a long, glowing wind-up. The game pauses briefly (slow-motion) on the first attack and shows the parry prompt. A successful parry staggers it; then the player finishes it.

| Who | Line |
|---|---|
| The Keeper | "You can't strike through that. Meet it. Turn its strength aside." |
| *Prompt* | *Parry* (F / LB / shield button), timed when the glow flashes |
| Caelan (after the parry) | "Still got it." |
| The Keeper | "Your Ember is cracking, Caelan. I can hear it." |

**Note for combat design:** the parry window here should be generous. This is where players learn it, not where they're tested. See the lag concern in [[GDD v0.2 Review]].

## Scene 4: The Void (about 60 s)
**Teaches:** the Ember skill, and that some fights can't be won alone.
**Setting:** the Shrine of Embers. A tall stone ring with an ember-fire burning inside. The sky above it tears open into darkness.

**Gameplay:**
1. The Ember skill unlocks: *Ember Strike* (E / Y / skill button). Caelan blasts back waves of Void tendrils. The player should feel powerful.
2. The tendrils keep coming. Each Ember Strike cracks Caelan's sword more and turns his glow from gold to purple.
3. Caelan's health and Ember bar drain no matter what the player does. The fight cannot be won. It ends after about 40 seconds or when the bar empties.

| Who | Line |
|---|---|
| The Void (deep, everywhere) | "Another champion. Another Ember. They always come back to me." |
| The Keeper | "Caelan, give it to me. Let me pass it on before it's too late." |
| Caelan | "If I let go, who holds the line?" |
| The Keeper | "Someone else. That's the point." |
| Caelan | "No. Not again. I can hold it." |
| The Void | "You all say that." |

## Scene 5: The fall (about 30 s, cutscene, skippable after first view)
**Camera:** Caelan drops to one knee. The darkness wraps around him. His armor turns black and cracked. His eyes glow purple.

| Who | Line |
|---|---|
| The Keeper (quietly) | "Oh, Caelan." |
| Fallen Caelan (voice changed) | "...I... hold... the line..." |

**The key moment:** as he falls, a small golden spark breaks free from his chest. The Keeper catches it in her hands. **This is the Ember that does not get corrupted.**

| Who | Line |
|---|---|
| The Keeper (to the spark) | "Not this one. This one I'll keep safe." |
| The Keeper | "Wait for the right heart. However long it takes." |

The spark rises into the sky. Time-lapse: Havenbrook is rebuilt, seasons pass, the shrine overgrows. *One hundred years later.*

## Scene 6: Present day (about 45 s)
**Setting:** present-day Havenbrook at dawn. Warm sunlight, stone streets, banners (GDD visual direction). The player's own character lies in the overgrown Shrine of Embers.

**Gameplay:**
1. **Character creation** happens here: the screen fades from gold, the player makes their character, and the camera opens on them waking up.
2. The golden spark drifts down and settles into the player's chest. The Ember bar appears in the HUD, faint.
3. The Keeper is waiting at the shrine.

| Who | Line |
|---|---|
| The Keeper | "There you are. I've waited a long time." |
| The Keeper | "You carry what he would not let go." |
| Player (choice, flavor only) | **A:** "Who?" · **B:** "Carry what?" · **C:** *(say nothing)* |
| The Keeper | "A champion's Ember. And with it, his fight." |
| The Keeper | "The Void is stirring again. It always does." |
| The Keeper | "Go to the guild hall. Tell them the Keeper sent you." |
| The Keeper (as player leaves) | "And when your time comes... come back here. Don't hold on too long." |

**First quest given:** *A Spark in Havenbrook*, go to the guild hall. Act 1 begins.

That last Keeper line foreshadows retirement. Players won't understand it now; they will when the Shrine of Embers becomes the retirement point.

---

## Payoffs later in the game
- **Fallen Knight boss fight (Act 1):** he uses Caelan's moves from the prologue, and fights the player's Ember with his corrupted one. Mid-fight line: *"I... hold... the line."* His boss memory replays Scene 4 from the Keeper's view.
- **The wounded knight choice (Act 1):** saving the wounded knight is the chance to do what the Keeper could not do for Caelan. Leaving him lets him fall the same way.
- **Retirement (later):** the Keeper runs the ceremony. Her line on the player's first retirement: *"You let go. He never could."*

## Heirs (later generations)
When a new generation starts, the full prologue is skipped. The heir gets a 20-second variant: the Keeper hands the family's Ember from the retired character to the heir at the Shrine of Embers. Not needed in the first playable build.

## Implementation notes (for the gameplay programmer)
- **Where it runs:** build it as a hidden zone inside the Havenbrook place, not as a separate place. That avoids a teleport in a new player's first minute, which is the riskiest moment for a failed teleport (see [[GDD v0.2 Review]]).
- **Solo:** enemies in the prologue give no rewards, so they can be simulated on the client. This keeps it solo without a reserved server. Shortcut with a known limit: client-side enemies are only fine because nothing here is saved or rewarded.
- **Save flag:** store `bloodline.familyFlags.prologue_seen = true` when the player reaches Scene 6, as set out in the [[Legacy System Spec]] data model.
- **Disconnect:** if the player leaves mid-prologue, they restart it from the beginning. It's short enough for that to be acceptable.
- **Assets needed:** burning old Havenbrook (a damaged copy of the Havenbrook blockout), north gate, Shrine of Embers (old and overgrown versions), Caelan model, Fallen Caelan model (reused as the Act 1 boss), hollowed soldier, Void Brute, the Keeper, Void tendrils VFX, Ember VFX.

## Open questions
- [ ] Is "Caelan" the final name for the champion?
- [ ] Voice acting or text only? Text with sound effects is cheaper; voice for the cold open only is a middle option.
- [ ] Should character creation come before the prologue instead, so players feel they are someone from the start?
- [ ] Is the "Tomas" line too dark for the Roblox audience?
