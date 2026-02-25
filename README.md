# Custom War Room

> A Claude Code skill that builds reusable war rooms — 3 world-class experts who wage total war over your topic and produce a battle-tested verdict.

---

## What it does

**Custom War Room** lets you create, save, and reuse panels of 3 top-tier experts who debate any topic until they reach genuine consensus — or an honest truce.

It operates in three modes:

| Mode | What happens |
|---|---|
| **Create** | Generates 3 expert profiles + config in a reusable folder. Debate starts immediately. |
| **Invoke** | Loads an existing war room folder and starts the debate with preloaded profiles. |
| **One-shot** | Ephemeral debate — nothing saved, just raw war. |

The skill detects the mode automatically from your message. No configuration needed.

---

## How it works

1. **Profile selection** — The skill picks 3 absolute titans of the domain. Not "someone who works in the field" — THE person whose name IS the field. Their philosophies must genuinely clash.
2. **War** — Experts debate in rounds. They interrupt, misquote each other on purpose, form alliances and break them. Hidden agendas surface as rounds progress. Maximum 10 rounds.
3. **Verdict** — Each expert declares their final position and what they had to swallow to get there.
4. **Consensus** — Actionable summary of what was agreed, or the truce if no full agreement.

---

## Expert selection

Profiles follow a strict hierarchy:

1. **Top-tier, world-class famous figures** — founders, Nobel laureates, legendary CEOs, iconic creators. If you have to explain who they are, pick someone bigger.
2. **Hyper-specific fictional archetypes** — only if no suitable real titans exist. Never generic, never B-list.

Validation test: *Would these three people professionally despise each other?* If not, change them.

---

## Installation

### Claude Code — project only
```bash
cp -r war-room-builder/ .claude/skills/custom-war-room/
```

### Claude Code — global (all projects)
```bash
cp -r war-room-builder/ ~/.claude/skills/custom-war-room/
```

### skills.sh
```bash
npx skills add ericrisco/war-room-builder
```

---

## Usage

### Create a persistent war room
```
Create a war room to analyze LinkedIn content strategy
```

### Activate an existing war room
```
Activate my war room at ./brand-war-room about this document
```

### Ephemeral debate
```
War panel on whether we should migrate to microservices
```

---

## Generated war room structure

Each persistent war room creates a self-contained folder:

```
my-war-room/
├── war-room.config.json    # metadata: name, topic, mode
├── profiles/
│   ├── expert-1.md         # expert profile 1
│   ├── expert-2.md         # expert profile 2
│   └── expert-3.md         # expert profile 3
└── SKILL.md                # standalone installable skill
```

Each generated war room works as an independent skill — install it, share it, version it.

---

## Debate rules

These are sacred. They apply always, without exception:

- They interrupt without asking permission
- They deliberately misquote each other
- They change positions if the argument warrants it
- They form alliances and break them when convenient
- They have hidden agendas that surface as rounds progress
- Maximum 10 rounds, genuine consensus or honest truce
- If consensus comes early, it ends early — no padding

---

## Project structure

```
custom-war-room/
├── SKILL.md                    # Main skill file (instructions for Claude)
├── references/
│   └── profile-template.md     # Expert profile template
├── README.md
└── LICENSE
```

---

## License

MIT — see [LICENSE](LICENSE) for details.
