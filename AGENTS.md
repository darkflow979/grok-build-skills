# AGENTS.md

Orientation for any coding or research agent working **in this repository**.

## What this repo is

A **Meta-Skill Capture System**: Markdown + Git only.  
Successful agent workflows are distilled into reusable, versioned skills under `skills/`.

This is **not** an application, not an Obsidian vault, and not a runtime skill installer.

## Layout (load-bearing paths)

| Path | Role |
|------|------|
| `README.md` | Human/model front door |
| `docs/skill-format.md` | Authoritative skill file spec |
| `docs/capture-workflow.md` | How to capture a session into a skill |
| `templates/SKILL.template.md` | Starting point for new skills |
| `skills/INDEX.md` | Discovery catalog (must stay accurate) |
| `skills/<name>/SKILL.md` | Individual skills |

## Rules when editing

1. **Prefer distillation over transcripts.** Skills are procedures, not chat logs.
2. **One skill = one folder** named like the frontmatter `name` field.
3. **Every new or renamed skill updates `skills/INDEX.md`** in the same change.
4. **Follow `docs/skill-format.md`** for frontmatter and section order.
5. **Start new skills as `status: draft`** unless the operator asks for `active`.
6. **No secrets** in skills (tokens, keys, private personal data).
7. **Do not add automation** (scripts, capture pipelines, SkillOpt harnesses, gateway/router logic) unless the user explicitly requests it.
8. **Do not assume this lives inside Driftworks or any Obsidian vault.** Keep the repo self-contained.
9. **Prefer updating an existing skill** over creating a near-duplicate.
10. **Bump `version` and `updated`** when the procedure changes meaningfully.

## Adding a skill (short path)

1. Copy `templates/SKILL.template.md` → `skills/<name>/SKILL.md`
2. Fill frontmatter + body sections
3. Add a row to `skills/INDEX.md`
4. Point the user at the new paths for review

Full workflow: `docs/capture-workflow.md`  
Executable skill form: `skills/capture-session-to-skill/SKILL.md`

## Out of scope unless asked

- Automatic capture from session logs
- Trajectory logging / eval harnesses
- Full SkillOpt integration
- Agent gateway or router logic
- Obsidian plugins or vault-coupled tooling
