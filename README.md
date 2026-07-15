# grok-build-skills

**A Meta-Skill Capture System for turning successful agent sessions into reusable, versioned skills—using only Markdown and Git.**

This repository is a standalone skill library and capture workflow. It is **not** an Obsidian vault, not a plugin, and not an automatic logging system. It is designed so humans and coding agents (Grok Build, Claude Code, Codex, Cursor, and others) can read, extend, and apply skills with minimal ceremony.

> **Principle:** Memory before automation. Capture well by hand first; automate later if the process earns it.

---

## Why this exists

Coding agents are good at one-off execution and bad at institutional memory. The same hard-won procedure—how to scaffold a clean repo, how to verify a change, how to distill research—gets re-discovered in chat and lost again.

This repo treats skills as **small, reviewable units of procedure**:

| Goal | How this repo helps |
|------|---------------------|
| Reliable agents | Skills encode steps, constraints, and failure modes—not vibes |
| Reuse | Successful runs become durable files under `skills/` |
| Research | Versioned skills support essay work and later SkillOpt-style optimization |
| Portability | Core format aligns with open Agent Skills-style `SKILL.md` files |
| Reviewability | Everything is Markdown + Git; easy to diff and edit |

---

## What v1 includes (and excludes)

### In scope

- Clean folder structure
- This README and supporting docs
- Reusable skill template
- Skill index for discovery
- Documented capture workflow
- One example skill (`capture-session-to-skill`)

### Out of scope (v1)

- Automatic capture scripts
- Trajectory logging
- Full SkillOpt integration
- Agent gateway or router logic
- Obsidian plugin code
- Vault coupling (this repo is standalone)

---

## Repository layout

```text
grok-build-skills/
├── README.md                 # You are here
├── AGENTS.md                 # Short norms for agents editing this repo
├── LICENSE                   # MIT
├── .gitignore
├── docs/
│   ├── capture-workflow.md   # Full capture workflow
│   └── skill-format.md       # Frontmatter + body specification
├── templates/
│   └── SKILL.template.md     # Copy this to start a new skill
└── skills/
    ├── INDEX.md              # Browse / discover skills
    └── capture-session-to-skill/
        └── SKILL.md          # Example skill (dogfoods the system)
```

---

## Capture workflow (summary)

Full detail: [docs/capture-workflow.md](docs/capture-workflow.md).  
Executable form: [skills/capture-session-to-skill/SKILL.md](skills/capture-session-to-skill/SKILL.md).

1. **Finish a successful session** (or select a proven repeated procedure).
2. **Decide it is worth capturing** — repeatable, non-trivial, transferable; not a secret-laden one-off.
3. **Name the skill** — `kebab-case`, matches the folder name.
4. **Copy the template** — `templates/SKILL.template.md` → `skills/<name>/SKILL.md`.
5. **Distill** — rewrite goals, steps, constraints, and failure modes. Do **not** paste the chat transcript.
6. **Fill frontmatter** — at least `name` + `description`; set `status: draft` until reviewed.
7. **Update** [skills/INDEX.md](skills/INDEX.md).
8. **Human review** — cold-start clarity, no secrets, honest “when not to use.”
9. **Promote** to `status: active` when ready.
10. **Optional** — manually export a slim `SKILL.md` to a runtime skills directory (see below).

---

## Skill file format (summary)

Authoritative spec: [docs/skill-format.md](docs/skill-format.md).

Each skill lives at:

```text
skills/<skill-name>/SKILL.md
```

### Frontmatter (required core)

```yaml
---
name: skill-name
description: >
  What it does and when to use it. Include trigger phrases.
---
```

### Frontmatter (recommended in this repo)

```yaml
status: draft          # draft | active | deprecated
version: 0.1.0
tags: [example]
domain: meta           # e.g. meta, writing, research, engineering
source: grok-build     # grok-build | claude-code | codex | cursor | manual | mixed
created: YYYY-MM-DD
updated: YYYY-MM-DD
triggers:
  - example phrase
```

### Body sections (in order)

1. Purpose  
2. When to use  
3. When not to use  
4. Inputs  
5. Procedure  
6. Output  
7. Failure modes  
8. Examples  
9. Provenance (optional)

The required core (`name`, `description`, procedure-oriented body) is compatible with open [Agent Skills](https://agentskills.io)-style runtimes. Extra fields are this repo’s capture/research layer.

---

## Discovering skills

**Primary method:** open [skills/INDEX.md](skills/INDEX.md).

The index is a hand-maintained table (name, status, domain, tags, short description) plus optional domain groupings. Keep it accurate whenever you add or change a skill.

**Other options:**

- Search the repo for `SKILL.md` files or frontmatter `name:` lines
- Point an agent at `skills/INDEX.md` and ask it to pick a skill for a task

v1 does not require Obsidian or Dataview. If skills are later mirrored into a vault, the same YAML frontmatter can support richer queries there.

---

## Contributing a new skill

1. Read [docs/skill-format.md](docs/skill-format.md) and [docs/capture-workflow.md](docs/capture-workflow.md).
2. Copy [templates/SKILL.template.md](templates/SKILL.template.md) to `skills/<name>/SKILL.md`.
3. Fill frontmatter and all standard sections.
4. Add a row to [skills/INDEX.md](skills/INDEX.md).
5. Prefer `status: draft` until reviewed.
6. Open a PR or commit with a clear message (e.g. `Add skill: <name>`).

### Quality checklist

- [ ] Another agent could follow **Procedure** without the original chat  
- [ ] **When not to use** is honest  
- [ ] No secrets or private credentials  
- [ ] `name` matches folder name  
- [ ] INDEX row added/updated  
- [ ] Description includes real triggers  

Agents editing this repo should also follow [AGENTS.md](AGENTS.md).

---

## Using skills with agents

### In this repository

Point the agent at a specific skill:

```text
Follow skills/capture-session-to-skill/SKILL.md to capture the last session.
```

Or ask it to choose from the index:

```text
Read skills/INDEX.md and apply the best skill for <task>.
```

### In a runtime skill directory (manual export)

This repo is the **source of truth**. Runtime directories (examples) are separate:

| Target | Typical path |
|--------|----------------|
| Grok user skills | `~/.grok/skills/<name>/SKILL.md` |
| Grok project skills | `<repo>/.grok/skills/<name>/SKILL.md` |
| Other agents | Whatever path that product documents for skills |

For export, keep at least `name`, `description`, and the procedure-oriented body. You may drop capture-only fields and Provenance. **v1 does not automate install or sync.**

---

## Future: Driftworks / Obsidian

This repository is intentionally **standalone**. It does not live inside the Driftworks vault and does not depend on Obsidian.

Possible later integrations (not implemented in v1):

- **Mirror or submodule** of `skills/` into Driftworks for PKM linking and essay research  
- **Dataview** (or similar) queries over skill frontmatter once mirrored  
- Skills as **operational appendices** to reliability / verification / SkillOpt notes  
- Clear boundary preserved: vault = thinking and writing system; this repo = skill source of truth  

No bidirectional sync and no vault plugins are planned for v1.

---

## Future: automation and SkillOpt

Named here so the design stays honest about growth paths—without building them yet:

| Idea | Role |
|------|------|
| Capture helpers | Scaffold a skill folder from a short form (still human-reviewed) |
| Trajectory logging | Attach eval traces to skill versions for research |
| SkillOpt-style loops | Propose skill edits from failure clusters; human still merges |
| Multi-agent sources | Same format for Grok, Claude Code, Codex, Cursor, etc. |
| Runtime adapters | Export packs tuned to each product’s skill loader |

Until those exist, improve skills by editing Markdown and reviewing diffs.

---

## License

[MIT](LICENSE)

---

## Quick start

```text
1. Clone or open this repo
2. Browse skills/INDEX.md
3. Open skills/capture-session-to-skill/SKILL.md
4. After your next good agent run, capture a new skill with the template
```

Questions about format → [docs/skill-format.md](docs/skill-format.md)  
Questions about process → [docs/capture-workflow.md](docs/capture-workflow.md)  
Questions for agents editing here → [AGENTS.md](AGENTS.md)
