<!--
version: 2.0
date: 2026-04-01
source: Update — SiteGround location, version header format, reconstruction pattern,
        250-line guidance nuanced (methodology lean, operational depth OK in v2.0+),
        deployment pattern, skill update workflow from actual sessions
-->

---
name: skill-creation
description: "Build, audit, or rewrite skills for the 8th Street / PathAcross skill library. Use when creating a new skill from scratch, improving an existing skill, auditing skills for agent-readiness, or restructuring a skill into core + reference files. Always use this skill when Jim says build a skill, update this skill, audit our skills, or when Claude Code is restructuring the skill library."
---

# Skill Creator
## The 8th Street Standard for Agent-Ready Skills

---

## Where Skills Live

All skill files: `~/skills/user/[skill-name]/SKILL.md` on SiteGround
(ssh.8thstreetsolutions.com, u3063-yznvsscqgtmo, port 18765)

Full path: `/home/u3063-yznvsscqgtmo/skills/user/`

**Never edit skill files in /mnt/skills/user/** — session-dependent mount, unreliable.
All skill edits go to SiteGround via SCP + Python script (see deployment pattern below).

---

## What a Skill Is

A skill is a folder with a SKILL.md file in it. That\'s it. It encodes methodology in
plain English so an agent or human can execute a process predictably without being
re-prompted every time.

Skills compound. Prompts evaporate.

---

## Version Header Format

Every SKILL.md begins with:

```
<!--
version: [X.Y]
date: [YYYY-MM-DD]
source: [brief note on what changed]
-->
```

Increment version on every meaningful edit. Minor fix = bump patch (1.0 → 1.1).
Major reconstruction = bump major (1.x → 2.0).

---

## The Non-Negotiable Standard

Every skill we build or update must pass this bar:

1. **Agent-first** — written for an agent caller, not a human reader
2. **Single-line description** — never wrapped to a second line (agents won\'t read it)
3. **Pushy description** — Claude undertriggers by default; descriptions must be specific
   and confident
4. **Output as contract** — the agent must know exactly what it will get from invoking
   this skill
5. **Reference files for bulk** — client data, examples, specs go in the folder, not in
   SKILL.md

**On length:** The 250-line target is for the lean core — methodology, not operational
detail. Skills that carry rich operational state (live agent status, known bugs, active
client data) will be longer. That is acceptable. The principle is: SKILL.md should be
methodology + the operational details that change how you execute. Pure reference data
(full client histories, long examples) belongs in subfolder files.

---

## Skill Folder Structure

```
skill-name/
  SKILL.md          ← required. Core methodology. Lean where possible.
  references/       ← optional. Docs loaded on demand.
  examples/         ← optional. Pattern-match targets.
  scripts/          ← optional. Deterministic executables.
```

SKILL.md is the routing brain. Reference files carry the weight. The agent loads only
what the current task needs.

---

## Writing the Description (80% of the work)

The description is the triggering mechanism. Most skills fail here.

**Single line. Always.** If a formatter wraps it, the agent loses the second line. Put
the entire description in quotes on one line.

**Must include:**
- What the skill produces (specific artifact or action types)
- Trigger phrases — exact words Jim or an agent would use
- When to use it even if not explicitly asked
- What it does NOT cover (prevents wrong-skill invocations)

**Bad:** `"Helps with competitive analysis"`
**Good:** `"Produces competitor landscape reports, market positioning maps, and keyword
gap analyses. Use when Jim asks who are the players, analyze our competitors, or what
are we missing. Always use when a new client vertical is being researched. Does not
cover paid ad competitive analysis — use google-ads for that."`

The description is a routing signal, not a label.

---

## Writing the Methodology Body

Five things the body needs:

1. **Reasoning, not just steps** — give Claude the frameworks and principles, not just
   a checklist. A skill with only linear steps breaks on edge cases. Reasoning helps
   Claude generalize.

2. **Output contract** — state exactly what the skill produces. Format, fields, length,
   file type.

3. **Edge cases explicit** — everything a human handles through common sense must be
   written down. Do not assume Claude will infer it.

4. **Example to pattern-match against** — one good example in the folder is worth more
   than three paragraphs of description.

5. **Composability** — if this skill is part of an agent pipeline, state what the output
   needs to look like for the next step to consume it.

---

## The Output Contract Pattern

Every skill should contain a section like this:

```
## What This Skill Produces
- Output type: [markdown / PHP file / JSON / report / etc.]
- Always includes: [field 1], [field 2], [field 3]
- Never includes: [what\'s out of scope]
- Handoff format: [what the next agent or step expects to receive]
```

---

## Skill Reconstruction Pattern

When reconstructing a skill from session knowledge (not just updating):

1. Read the current SKILL.md on SiteGround
2. Read claude.md for the relevant project
3. Identify what\'s in the current file vs. what\'s known from actual work sessions
4. Add: live operational details, current agent status, known bugs, real client data
5. Remove or update: outdated status markers, references to things that changed
6. Bump to next major version (e.g., 1.0 → 2.0)
7. Write via Python script → SCP → SSH execute
8. Verify: `head -6` for version header + `wc -l` for line count

This is the pattern used in the March–April 2026 skill library reconstruction pass.

---

## Deployment Pattern

Skills are written to SiteGround via the standard Python script workflow:

1. Write Python script locally with content as a variable
2. `scp -i ~/.ssh/id_ed25519 -P 18765 script.py u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com:/tmp/script.py`
3. `ssh ... "python3 /tmp/script.py"`
4. Verify: `head -6 ~/skills/user/[skill]/SKILL.md && wc -l ~/skills/user/[skill]/SKILL.md`

**Python string rules for skill content:**
- Use `\'` for apostrophes inside single-quoted PHP code examples
- Use `—` for em dashes
- Use `→` for arrows
- Always `encoding=\'utf-8\'` when opening files

---

## Audit Checklist (use when reviewing existing skills)

- [ ] Version header present and incremented
- [ ] Description is a single line
- [ ] Description includes trigger phrases
- [ ] Description is pushy — would it trigger when it should?
- [ ] Body contains reasoning, not just steps
- [ ] Output format is explicitly stated
- [ ] Edge cases are written down
- [ ] Operational details are current (agent status, live versions, active clients)
- [ ] At least one example exists (in file or referenced)
- [ ] Client data / bulk reference content is in reference files, not inline
- [ ] Skill does not duplicate another skill — scope is clean
- [ ] Known bugs section present if relevant

---

## When to Break a Skill Into Multiple Files

Break it up when:
- Client-specific data is mixed with methodology
- Multiple distinct domains are covered (each domain gets its own reference file)
- Examples are long enough to distract from the core logic
- The file is becoming unwieldy to read and navigate

Pattern: SKILL.md routes and reasons. Reference files carry the data.

```
foundation/
  SKILL.md              ← methodology, standards, execution order
  references/
    clients.md          ← all client notes
    verticals.md        ← vertical-specific patterns
    current-standards.md ← evolving Google/AI search standards
```

---

## Building a New Skill — Process

1. **Identify the workflow** — what repeatable process is this capturing?
2. **Write the description first** — get triggering right before writing anything else
3. **Write the output contract** — what does invoking this skill produce?
4. **Write the reasoning** — principles and frameworks, not just steps
5. **Add edge cases** — what would an experienced human know that Claude won\'t?
6. **Add or reference an example** — what does good output look like?
7. **Add version header**
8. **Deploy via SCP + Python script**
9. **Verify** — head + wc -l confirmation

---

## Updating an Existing Skill

- Read the current skill first (always from SiteGround, never from memory)
- Identify what\'s methodology vs. reference data — separate them if mixed
- Apply the audit checklist
- Preserve the skill name exactly
- Increment version in header
- Deploy and verify

---

## What Skills Are Not

- Not a place for client passwords or sensitive credentials
- Not a substitute for deterministic scripts — if you need guaranteed behavior, use a
  script in `scripts/`
- Not a prompt library — skills are reusable methodology, not one-off prompts
- Not documentation for humans only — if a human can read it but an agent can\'t execute
  it, it\'s not a skill yet

---

## The Neil Patel Clarity Check (for skills that produce content)

Before finalizing a skill that writes pages, copy, or messaging:

1. **What is this?** — Does the output state plainly what it is in the first block?
2. **Is this for me?** — Does it name the specific person/situation it\'s for?
3. **Can I trust it?** — Are trust signals placed next to the claims that need them?

---

*Skill Creator — 8th Street Solutions*
*The standard for every skill in this library.*
*Skills live at ~/skills/user/ on SiteGround. Never /mnt/skills/user/.*
