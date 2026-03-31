<!--
version: 2.1
date: 2026-04-01
source: Refactored into routing brain + subfiles. Content moved to clients.md,
        new-build.md, schema.md, standards.md, verticals.md.
-->

---
name: foundation
description: "Build, maintain, and grow any Foundation site — new builds, second homes, and resource sites for any business type. Claude Code reads this skill and executes. Three site types: Main Site (primary web presence), Second Home (parallel site alongside existing), Resource Site (public authority site with sponsor). Consolidates all site types: use this skill for every Foundation build including second-sites and community-resource builds. Client notes in clients/[client-id]/context.md on SiteGround."
---

# FOUNDATION
## The 8th Street Build and Growth System

Foundation is the core product: 40–60 page PHP sites, fast and clean, built to rank and
convert. No WordPress. No CMS. Every site ships from the same structure and standards.

---

## Session Start Rule

1. Read this file to orient
2. Read the subfile(s) for the current task (routing table below)
3. Read `clients/[client-id]/context.md` for the specific client
4. Execute. Do not ask questions the skill already answers.
5. Log every change in the client\'s `change-log.md`

**When to stop and flag to Jim:**
- Required client info missing from client file
- Decision has strategic or client-relationship implications
- Content going live on a public page needs human review
- Anything outside the defined scope of this skill

**Note on site type consolidation (March 2026):** `foundation-second-sites` and
`foundation-community-resource` are consolidated here. Do not look for separate skill
files — they live in new-build.md.

---

## Task Routing Table

| Task | Read |
|------|------|
| New site build (any type) | new-build.md |
| Site type details (Main / Second Home / Resource) | new-build.md |
| Client info, intake, active clients | clients.md |
| Schema, structured data, p.section-deck | schema.md |
| File structure, config.php, meta, PHP rules, deploy | standards.md |
| Vertical-specific patterns (law, medical, trade...) | verticals.md |
| Current Google/AI search standards | standards.md (Part 5) |

---

## Subfile Index

```
foundation/
  SKILL.md          ← this file — routing only
  clients.md        ← active client table, client intake, client file standard
  new-build.md      ← build process, site types, execution rules
  schema.md         ← schema standards, patterns, AI visibility
  standards.md      ← file structure, config, meta, PHP rules, deploy process
  verticals.md      ← vertical-specific patterns by business type
```

---

*Foundation — 8th Street Solutions*
*Methodology only. Client data in clients/[client-id]/ on SiteGround.*
