\---

name: capture-meta-skill-workflow

description: >

&#x20; Turn a successful Grok Build (or similar agent) session into a reusable, versioned skill in the grok-build-skills repository.

&#x20; Use when the user wants to distill a proven workflow, prompt pattern, or procedure into a durable skill for future reuse.

status: active

version: 0.1.0

tags: \[meta, capture, grok-build, skills, workflow]

domain: meta

source: mixed

created: 2026-07-15

updated: 2026-07-15

triggers:

&#x20; - dogfood the capture system

&#x20; - capture this workflow

&#x20; - turn session into skill

&#x20; - create reusable skill

\---



\### Purpose



Convert a valuable agent session into a clear, reviewable, reusable skill file that follows the repository's standards.



\### When to use



\- After a successful Grok Build session that produced useful structure, prompts, or patterns

\- When a repeatable procedure emerges during agent work

\- When the user explicitly wants to capture something for the skills library



\### When not to use



\- One-off or highly context-specific sessions with private data

\- Sessions that didn't produce clear value or reusable steps

\- When the content contains secrets or sensitive information



\### Inputs



\- Completed agent session (chat history, plan, generated files)

\- Decision that the procedure is worth capturing

\- Access to the grok-build-skills repo



\### Procedure



1\. Decide the skill has clear, repeatable value.

2\. Choose a concise, kebab-case name that matches the folder.

3\. Copy `templates/SKILL.template.md` into `skills/<name>/SKILL.md`.

4\. Fill frontmatter (especially `name`, `description`, `status: active` after review).

5\. Distill the session: focus on purpose, triggers, procedure, outputs, and failure modes. Avoid raw chat transcripts.

6\. Update `skills/INDEX.md` with a new row.

7\. Human review: clarity, honesty in "When not to use", no secrets.

8\. Commit with a clear message and push.



\### Output



\- New folder `skills/<name>/` containing `SKILL.md`

\- Updated `skills/INDEX.md`

\- Committed and pushed changes



\### Failure modes



\- Creating overly broad or vague skills

\- Including sensitive/private data

\- Forgetting to update INDEX.md

\- Keeping raw chat logs instead of distilled procedure



\### Examples



\*\*Example 1 (this skill)\*\*: The creation of the grok-build-skills repository itself using a detailed Plan Mode prompt.



\*\*Example 2\*\*: Turning a successful Zettelkasten ingestion session into a reusable literature note template skill.



\### Provenance



Created by distilling the initial build of the grok-build-skills repo on 2026-07-15.

