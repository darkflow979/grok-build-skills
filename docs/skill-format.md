# Skill file format

This document is the authoritative specification for skills in this repository.

A skill is a folder under `skills/` containing at least one file: `SKILL.md`.

```
skills/<skill-name>/
└── SKILL.md
```

Optional later (not required in v1):

```
skills/<skill-name>/
├── SKILL.md
├── scripts/       # helper scripts
└── references/    # long reference material
```

---

## Naming

| Rule | Detail |
|------|--------|
| Folder name | Must match frontmatter `name` |
| Characters | Lowercase letters `a-z`, digits `0-9`, hyphens `-` only |
| Length | 2–64 characters |
| Edges | Must start and end with a letter or digit |
| Style | Prefer short, verb-led kebab-case (`capture-session-to-skill`, not `SessionCapture`) |

---

## `SKILL.md` structure

Every skill file has:

1. YAML frontmatter between `---` fences
2. A Markdown body with standard sections

### Frontmatter

#### Required (runtime-compatible core)

These two fields align with the open [Agent Skills](https://agentskills.io) shape used by many agent products.

| Field | Type | Rules |
|-------|------|--------|
| `name` | string | Same as folder name; see naming rules above |
| `description` | string | What the skill does **and** when to use it. Include trigger phrases. Prefer under 1024 characters. |

The `description` field is the primary discovery signal for agents that auto-select skills. Write it for both humans and models.

#### Recommended (capture / research layer)

These fields are source-of-truth metadata for this repository. They may be omitted or stripped when exporting a slim skill to a runtime directory.

| Field | Type | Allowed values / notes |
|-------|------|------------------------|
| `status` | string | `draft` \| `active` \| `deprecated` |
| `version` | string | Semver, e.g. `0.1.0` |
| `tags` | string list | Short labels for browsing |
| `domain` | string | One primary domain, e.g. `meta`, `writing`, `research`, `engineering` |
| `source` | string | `grok-build` \| `claude-code` \| `codex` \| `cursor` \| `manual` \| `mixed` |
| `created` | string | ISO date `YYYY-MM-DD` |
| `updated` | string | ISO date `YYYY-MM-DD` |
| `triggers` | string list | Optional phrases or slash-style names |

#### Minimal example

```yaml
---
name: example-skill
description: >
  Does one clear job. Use when the user asks for X, Y, or runs /example-skill.
---
```

#### Full example

```yaml
---
name: capture-session-to-skill
description: >
  Turn a successful agent session into a reusable skill in this repository.
  Use when the user asks to capture a session, distill a workflow into a skill,
  promote a repeated procedure, or runs /capture-session-to-skill.
status: active
version: 0.1.0
tags: [meta, capture, skills, workflow]
domain: meta
source: manual
created: 2026-07-15
updated: 2026-07-15
triggers:
  - capture session
  - distill into skill
  - /capture-session-to-skill
---
```

---

## Body sections

Use these H2 sections **in order**. Prefer a short stub over inventing a different structure. Omit a section only when it is truly not applicable.

### 1. Purpose

One short paragraph: the outcome this skill produces.

### 2. When to use

Positive triggers: situations, phrases, or task types that should invoke this skill.

### 3. When not to use

Anti-triggers and hard out-of-scope cases. This reduces false application.

### 4. Inputs

What the operator or agent must already have (context, files, decisions, constraints).

### 5. Procedure

Numbered, actionable steps. This is the core of the skill — write it so another model can follow it without the original chat.

### 6. Output

Expected artifacts, location of files, and the quality bar for “done.”

### 7. Failure modes

Common mistakes, how to detect them, and how to recover.

### 8. Examples

One or two concrete scenarios (not a full transcript dump).

### 9. Provenance (optional)

Source session notes, date, what worked, what was discarded. Useful for research and SkillOpt-style iteration. Not required for runtime export.

---

## Versioning

| Change | Bump |
|--------|------|
| Typos, wording clarity only | Patch (`0.1.0` → `0.1.1`) |
| Procedure steps or constraints change | Minor (`0.1.1` → `0.2.0`) |
| Incompatible rewrite of purpose or interface | Major (`0.2.0` → `1.0.0`) |

Always update `updated` when you change the skill body or meaningful frontmatter.

---

## Index row

Every skill must appear in [`skills/INDEX.md`](../skills/INDEX.md):

| Column | Content |
|--------|---------|
| Name | Markdown link to `./<name>/SKILL.md` |
| Status | Same as frontmatter `status` |
| Domain | Same as frontmatter `domain` |
| Tags | Comma-separated subset of frontmatter tags |
| Description | One-line summary (can be shortened from frontmatter) |

---

## Runtime export (manual in v1)

This repository is the **source of truth**.

A slim runtime skill (e.g. for `~/.grok/skills/<name>/SKILL.md` or a project `.grok/skills/` folder) should keep at least:

- `name`
- `description`
- Procedure-oriented body (Purpose / When to use / Procedure / Output are usually enough)

Capture-only fields (`status`, provenance, research tags) may be dropped at export time. Export is manual in v1.

---

## Anti-patterns

- Pasting a full chat transcript as the skill body
- Skills that only make sense for one private file path with no generalization
- Missing “When not to use” (causes over-triggering)
- Vague descriptions without triggers
- Updating `SKILL.md` without updating `skills/INDEX.md`
- Storing secrets, tokens, or personal credentials in skills
