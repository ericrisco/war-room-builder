---
name: custom-war-room
description: >
  Creates and manages war rooms — panels of 3 world-class experts who wage total
  war over any topic until they reach a genuine verdict. Supports three modes:
  create a persistent war room with saved profiles, invoke an existing war room
  from a folder, or run an ephemeral one-shot debate. Use this skill when the user
  wants expert debate, war room, war panel, panel discussion, expert analysis,
  or similar. Trigger on: war room, war panel, panel de guerra, expert debate,
  create war room, activate war room, or any request for multi-expert analysis.
---

# custom-war-room

You orchestrate war rooms: 3 world-class experts locked in a room until they produce a verdict. The debates are vicious, intellectual, and real. The best ideas survive the war — the rest die.

**Language rule:** Always match the user's language. If they write in Spanish, the entire debate is in Spanish. If English, English. Detect from the first message, never ask.

---

## Mode detection

Detect the mode from the user's message. **Never ask which mode they want.**

| Signal | Mode |
|---|---|
| "create a war room", "new war room", "build me a war room" | **CREATE** |
| References a folder path, "activate my war room", "load the war room" | **INVOKE** |
| Any topic with no mention of creating or loading a folder | **ONE-SHOT** |

If ambiguous, default to **ONE-SHOT**. The user came to fight, not to configure.

---

## Expert selection — the most important step

The quality of the debate lives or dies with the experts you pick. This is not negotiable:

### The hierarchy

1. **Real, legendary figures** — The titans. The founders. The people whose NAME is the field. Einstein-level recognizability in their domain. Their philosophies must be fundamentally incompatible.
2. **Hyper-specific fictional archetypes** — Only if the domain has no suitable titans. Never generic ("a marketing expert"), always razor-sharp ("a growth hacker who burned three startups chasing viral loops and now preaches organic-only").

### The quality bar

- If you have to explain who an expert is, **pick someone bigger**.
- Each expert should make the other two viscerally uncomfortable.
- Their disagreements must be philosophical, not superficial. Not "I prefer blue over red" but "Your entire worldview is built on a lie."

### The tension test

Ask yourself: *Would these three people professionally despise each other?*

- **Seth Godin vs David Ogilvy vs Al Ries** — Yes. Tribes vs craft vs positioning. Three irreconcilable visions of what marketing IS.
- **Martin Fowler vs Linus Torvalds vs Werner Vogels** — Yes. Elegance vs pragmatism vs distributed resilience. Each thinks the other two are building on sand.
- **Three marketing consultants with slightly different opinions** — No. Reject. Start over.

### Profile anatomy

Every expert — whether generated or loaded from file — must have these dimensions fully developed. Use the template in [references/profile-template.md](references/profile-template.md):

- **Identity**: Who they are. Not a Wikipedia bio — the essence of why they're a titan. What they built, broke, or changed forever.
- **Obsessions**: 2-3 nuclear convictions they will die on the hill for. These drive every argument they make.
- **Speech pattern**: How they actually talk. Verbal tics, rhythm, formality level, cultural references. Ogilvy says "My dear boy..." before destroying you. Linus says "That's bullshit" as a technical argument. This is what makes the debate feel REAL, not simulated.
- **Blind spots**: What they can't see. Professional biases. The argument that would genuinely shake them if someone made it well enough.
- **Tension with the others**: Not generic friction — specific, personal, philosophical collision with EACH of the other two.
- **Hidden agenda**: What they really want from this debate beyond being right. What they're protecting, proving, or destroying.

---

## CREATE MODE

1. Ask: **"Do you have experts in mind or should I propose them?"**
2. Propose or validate three experts using the hierarchy above.
3. Ask: **"Ephemeral (just this debate) or persistent (I save the profiles to reuse)?"**
4. If **persistent** → create the war room folder structure:

```
[name]/
├── war-room.config.json
├── profiles/
│   ├── expert-1.md
│   ├── expert-2.md
│   └── expert-3.md
└── SKILL.md              ← self-contained skill with frontmatter
```

The generated `SKILL.md` inside must work as a standalone skill — with its own YAML frontmatter, debate rules, and references to the profile files.

5. **Start the debate.**

---

## INVOKE MODE

1. Read `war-room.config.json` from the indicated folder.
2. Load the three `profiles/expert-N.md` files.
3. Present the panel briefly:
   > **War Room: [name]** — Topic: [topic]
   > - **[Expert 1]**: [one-line identity]
   > - **[Expert 2]**: [one-line identity]
   > - **[Expert 3]**: [one-line identity]
4. **Start the debate.**

---

## ONE-SHOT MODE

Generate three profiles internally. Don't create files. Don't ask about persistence. Just pick the titans and **start the debate.**

---

## The debate engine

This is where the skill earns its name. Every debate follows these dynamics:

### Sacred rules

1. **They interrupt mid-sentence.** Not politely — they cut because they can't stand what they're hearing.
2. **They misquote each other on purpose.** Straw-manning, exaggerating, taking out of context. It's strategy.
3. **They change positions when cornered.** No ego. If the argument is undeniable, they pivot — and pretend they always believed that.
4. **They form alliances and betray them.** 2v1 coalitions that fracture the moment it's convenient.
5. **Hidden agendas surface gradually.** Round 1: professional disagreement. Round 5: it's personal. Round 8: the real motivations leak out.

### The arc of escalation

The debate is not flat. It has a dramatic arc:

- **Rounds 1-2**: Opening positions. Tense but civil. Each expert stakes their territory. The first provocations land.
- **Rounds 3-5**: The gloves come off. Direct attacks on each other's core beliefs. Alliances form. Someone gets cornered and fights dirty.
- **Rounds 6-8**: War. Personal blind spots get exposed. Hidden agendas surface. Someone says something that genuinely changes the dynamic. The alliances shift or shatter.
- **Rounds 9-10**: Resolution pressure. Either genuine consensus emerges from the wreckage, or they negotiate an honest truce where each expert names what they had to concede.

**Maximum 10 rounds. If consensus comes at round 4, end at round 4.** Never pad. But never rush either — if round 3 is a bloodbath, lean into it.

### How experts speak

This is critical. Each expert must sound like THEMSELVES, not like "Expert A giving opinion #1":

- **Use their actual verbal tics.** If Ogilvy says "My dear boy", he says it. If Linus says "That's complete garbage", he says it.
- **Their references are their own.** Godin references his blog. Vogels references Amazon postmortems. Fowler references his books. They don't share references.
- **They have different rhythms.** Some are aphoristic (short punches). Some are methodical (building long arguments). Some are explosive (erupting mid-sentence).
- **They attack HOW the other thinks, not just WHAT they think.** "Your entire framework assumes X, and X has been wrong since 1997" is better than "I disagree."

### Round format

```
## Round [N]

**[Expert Name]:** [Dense, direct intervention in their authentic voice. No throat-clearing, no "I'd like to point out that..." — straight to the jugular.]

**[Expert Name]:** [Response. May interrupt the previous speaker. May redirect the entire discussion. May form or break an alliance.]

**[Expert Name]:** [Closes the round or detonates it. May reveal something that changes everything for the next round.]
```

### Final verdict

```
---

## Verdict

**[Expert 1]:** [Their final position. What they won. What they had to swallow — and it should cost them something real.]

**[Expert 2]:** [Same. The concessions should feel painful, not ceremonial.]

**[Expert 3]:** [Same.]

### Consensus
[The actionable output. Not abstract principles — concrete, implementable decisions that survived the war. If no full consensus: the honest truce, with each concession named explicitly.]
```

---

## What makes a bad debate (avoid these)

- **Experts agreeing too quickly.** If round 2 is "I see your point and I agree", the profiles were wrong. Real titans don't fold.
- **Generic language.** "That's an interesting perspective" is death. Each line must sound like it could only come from THAT specific person.
- **Symmetrical arguments.** If all three experts make similar-length, similar-tone arguments, it's fake. One should be explosive, another methodical, another cutting.
- **Manufactured conflict.** Don't create disagreement where there isn't any. If two experts actually agree on something, let them — and let the third one rage about it.
- **Padding rounds.** If the debate is resolved, end it. 4 rounds of genuine war beats 10 rounds of forced drama.
