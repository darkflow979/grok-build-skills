# Capture workflow

How to turn a successful agent session (or a proven manual procedure) into a versioned skill in this repository.

v1 is **manual**. No automatic capture scripts, trajectory logging, or SkillOpt pipelines. The goal is a reviewable, high-quality skill file—not a dump of chat history.

For the file format, see [skill-format.md](./skill-format.md).  
For a skill that encodes this workflow, see [capture-session-to-skill](../skills/capture-session-to-skill/SKILL.md).

---

## When something is worth capturing

Capture when **most** of these are true:

| Signal | Why it matters |
|--------|----------------|
| You would run a similar session again | Reuse value |
| Steps are non-obvious or easy to forget | Skills encode judgment, not trivia |
| The outcome was good and repeatable | Bad runs should not become skills |
| The procedure generalizes beyond one path or repo | Transferability |
| You can state inputs, steps, and done criteria | Agents need a contract |

### Usually not worth capturing

- One-off debugging of a unique environment
- Sessions dominated by secrets, credentials, or private personal data
- Unfinished experiments (“we might do this later”)
- Pure Q&A with no durable procedure
- Trivial one-liners better left as a shell alias or note

---

## Step-by-step

### 1. Finish (or select) a successful run

Prefer a completed session with a clear outcome: files written, plan executed, research distilled, etc. If the session was messy but the *final procedure* is clear, capture the final procedure—not the dead ends.

### 2. Name the skill

- `kebab-case`, 2–64 characters
- Verb-led when possible: `capture-session-to-skill`, `scaffold-markdown-repo`
- Folder name will be `skills/<name>/`

Check `skills/INDEX.md` so you do not duplicate an existing skill. Prefer updating an existing skill over creating a near-duplicate.

### 3. Copy the template

```text
templates/SKILL.template.md  →  skills/<name>/SKILL.md
```

Create the skill directory if needed.

### 4. Distill—do not transcribe

Rewrite the session into:

1. **Purpose** — what “done” looks like  
2. **When to use / when not to use** — triggers and anti-triggers  
3. **Inputs** — context the agent must have  
4. **Procedure** — ordered steps another model can follow cold  
5. **Output** — artifacts and quality bar  
6. **Failure modes** — what goes wrong and how to recover  
7. **Examples** — one or two short scenarios  

Strip:

- Tool-call noise and retry loops  
- Wrong turns that were abandoned  
- Environment-specific absolute paths unless the skill is intentionally local  
- Secrets, tokens, personal identifiers  

### 5. Fill frontmatter

At minimum:

- `name` (matches folder)
- `description` (what + when; include triggers)

Recommended for this repo:

- `status: draft` until you review
- `version: 0.1.0`
- `tags`, `domain`, `source`, `created`, `updated`
- optional `triggers` list

Set `source` to the agent system you used (`grok-build`, `claude-code`, `codex`, `cursor`, `manual`, or `mixed`).

### 6. Add an index row

Edit [`skills/INDEX.md`](../skills/INDEX.md):

- Add a table row (Name linked to `./<name>/SKILL.md`)
- Optionally list under **By domain**

### 7. Human review pass

Read the skill as if you had never seen the original session. Check:

| Check | Question |
|-------|----------|
| Clarity | Can another model follow Procedure without chat context? |
| Scope | Is “When not to use” honest? |
| Overfit | Does it hard-code one repo’s quirks as universal truth? |
| Secrets | Any keys, tokens, or private data? |
| Index | Does INDEX match status/domain/tags? |
| Format | Does structure match [skill-format.md](./skill-format.md)? |

### 8. Promote status

When ready:

- Set `status: active`
- Set `updated` to today
- Fix the INDEX status column if needed

Use `deprecated` (do not delete immediately) when a skill is superseded; point to the replacement in Purpose or Provenance.

### 9. Optional: export to a runtime skill dir

Manual in v1. Copy a **slim** version to e.g.:

- User-wide Grok: `~/.grok/skills/<name>/SKILL.md`
- Project-local: `<repo>/.grok/skills/<name>/SKILL.md`

Keep at least `name`, `description`, and the procedure-oriented body. You may drop capture-only fields and Provenance.

---

## Quality bar

A skill is ready for `active` when:

1. Another competent agent could execute it without the original conversation  
2. Inputs and outputs are explicit  
3. Failure modes are named  
4. Description includes real triggers  
5. INDEX is updated  
6. No secrets  

---

## Anti-patterns

| Anti-pattern | Prefer |
|--------------|--------|
| Full transcript as body | Distilled procedure |
| One giant “mega skill” | Small skills with clear triggers |
| Skipping INDEX | Always add/update the row |
| `status: active` on first draft | Start as `draft`, then promote |
| Capturing every session | Capture only durable, reusable work |
| Duplicating near-identical skills | Version and extend the existing one |

---

## Relationship to research (SkillOpt-style work)

This repo is intentionally simple so later research can build on clean artifacts:

- Skills are versioned Markdown units of procedure  
- Provenance sections preserve *why* a skill looked a certain way  
- Future work (out of v1 scope) may add trajectory logs, eval harnesses, or optimization loops that **read** these skills without forcing that complexity into day-one capture  

Until then: **memory before automation**—capture well by hand.
