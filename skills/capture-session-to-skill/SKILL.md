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
  - promote procedure to skill
  - /capture-session-to-skill
---

# Capture session to skill

## Purpose

Produce a reviewable, versioned skill under `skills/<name>/SKILL.md` (plus an index row) that another human or agent can follow without the original chat transcript.

## When to use

- A session produced a clear, repeatable procedure worth reusing
- The user says “capture this,” “turn this into a skill,” or “distill this workflow”
- A procedure has been run successfully more than once and should be standardized
- You are promoting a draft procedure from notes into this repository

## When not to use

- One-off debugging with no transferable steps
- Sessions full of secrets, credentials, or private personal data
- Unfinished experiments without a stable procedure
- Pure Q&A with no durable workflow
- When an existing skill already covers the same job—update that skill instead

## Inputs

- Access to this repository (or a clear target path for new skills)
- Enough context from the successful session: goal, steps that worked, constraints, final artifacts
- A proposed skill name in `kebab-case` (or enough detail to choose one)
- Optional: which agent system produced the run (`grok-build`, `claude-code`, `codex`, `cursor`, `manual`, `mixed`)

## Procedure

1. **Eligibility check**  
   Confirm the work is repeatable, non-trivial, and generalizable. If not, stop and say why it should not become a skill.

2. **Name**  
   Choose `skills/<name>/` with naming rules from `docs/skill-format.md`. Search `skills/INDEX.md` for duplicates or near-duplicates.

3. **Scaffold**  
   Copy `templates/SKILL.template.md` to `skills/<name>/SKILL.md`. Create the directory if needed.

4. **Distill**  
   Rewrite the session into the standard body sections (Purpose through Examples). Encode the final working procedure—not every dead end. Remove tool noise, absolute private paths (unless intentional), and secrets.

5. **Frontmatter**  
   Set `name` to match the folder. Write a `description` that states **what** and **when** (include triggers). Set `status: draft`, `version: 0.1.0`, `created`/`updated`, `domain`, `tags`, and `source`.

6. **Index**  
   Add a row to `skills/INDEX.md` (and optionally under By domain).

7. **Self-review**  
   Read the skill cold. Verify another model could execute Procedure with only the skill file and repo docs. Fix overfit, missing anti-triggers, and vague outputs.

8. **Promote (when ready)**  
   After human review, set `status: active`, refresh `updated`, and align the INDEX status column.

9. **Optional export**  
   If asked, produce a slim runtime copy (`name` + `description` + procedure-oriented body) for a runtime skills directory. Do not invent export paths; ask or use paths the user provides.

## Output

- `skills/<name>/SKILL.md` following `docs/skill-format.md`
- Updated `skills/INDEX.md`
- Skill starts as `draft` unless the user explicitly wants `active` immediately
- Quality bar: cold-start executable procedure, no secrets, honest “When not to use”

## Failure modes

| Failure | Detection | Recovery |
|---------|-----------|----------|
| Transcript dump | Body reads like a chat log | Rewrite into ordered Procedure; delete noise |
| Duplicate skill | INDEX or name collision | Merge into existing skill; bump version |
| Missing INDEX row | New folder not listed | Add row before finishing |
| Overfit to one repo | Hard-coded paths or product-only steps presented as universal | Generalize; put local notes under Inputs or Provenance |
| Secrets leaked | Tokens, keys, private emails in file | Remove immediately; rewrite examples |
| Vague description | No triggers or “when to use” | Expand description and When to use |

## Examples

### Example 1 — After a successful Grok Build session

You scaffolded a clean Markdown repo with a README, template, and workflow doc. The user says: “Capture this as a skill.”

- Name: e.g. `scaffold-markdown-skill-repo` (only if the procedure is general enough)
- Distill steps that apply next time, not the one-off discussion about folder names
- Add INDEX row with `status: draft` until reviewed

### Example 2 — Repeated research distillation

You have three times turned messy notes into a structured brief with the same section order. Capture that section order and quality bar as a skill (e.g. under domain `research`), not the content of any single brief.

## Provenance

- Created as the first example skill for the `grok-build-skills` repository (v1).
- Encodes the manual capture workflow documented in `docs/capture-workflow.md`.
- Intentionally model-agnostic: works for Grok Build and other agent systems.
