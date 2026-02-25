# custom-war-room

Claude skill for creating and managing war panels with three experts who debate until they reach a verdict.

## What it does

1. **Create** custom war rooms: generates three expert profiles and config files in a reusable folder.
2. **Invoke** an existing war room: references its folder and starts the debate with preloaded profiles.
3. **One-shot**: ephemeral debate without saving anything.

## Installation

```bash
# Via Skills.sh
skills install custom-war-room
```

## Quick usage

### Create a persistent war room
```
"Create a war room to analyze LinkedIn content strategy"
```

### Activate an existing war room
```
"Activate my war room at ./brand-war-room about this document"
```

### Ephemeral debate
```
"War panel on whether we should migrate to microservices"
```

## Generated war room structure

```
my-war-room/
├── war-room.config.json    ← metadata: name, topic, mode
├── profiles/
│   ├── expert-1.md         ← expert profile 1
│   ├── expert-2.md         ← expert profile 2
│   └── expert-3.md         ← expert profile 3
└── SKILL.md                ← standalone installable skill
```

## Expert selection

Profiles follow a strict hierarchy:

1. **Globally recognizable real famous people** whose philosophies genuinely clash
2. **Hyper-specific fictional archetypes** if no suitable famous people exist

Validation test: *Would these three people professionally despise each other?* If not, change them.

## Debate rules

- They interrupt without asking permission
- They deliberately misquote each other
- They change positions if the argument warrants it
- They form alliances and break them when convenient
- They have hidden agendas that surface as rounds progress
- Maximum 10 rounds, genuine consensus or honest truce

## Included examples

- [`examples/brand-war-room/`](examples/brand-war-room/) — Seth Godin vs David Ogilvy vs Al Ries on brand strategy
- [`examples/tech-war-room/`](examples/tech-war-room/) — Martin Fowler vs Linus Torvalds vs Werner Vogels on software architecture

## Repository structure

```
custom-war-room/
├── SKILL.md                ← base instructions for Claude
├── references/
│   └── profile-template.md ← expert profile template
├── examples/
│   ├── brand-war-room/     ← working example
│   └── tech-war-room/      ← working example
└── README.md
```

## License

MIT
