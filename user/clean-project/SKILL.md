<!--
version: 2.0
date: 2026-04-01
source: Update — correct claude.md and skills paths, SSH connection details, skills
        directory in project table, next-session kickoff format strengthened, skill
        reconstruction as a cleanup output type, clean-project as end-of-cycle discipline
-->

---
name: clean-project
description: "Clean and tidy any 8th Street, client site, or PathAcross project at the end of a work cycle. Use when Jim says clean this project, tidy up, lets do a cleanup, or when multiple threads of work have accumulated and things need to be consolidated. Covers reading recent threads, updating skills and claude.md files, auditing public_html for junk and zip files, and producing ready-to-use updated files plus a checklist of findings. Also use to close a session cleanly after a major build or content cycle."
---

# Clean Project
## End-of-Cycle Discipline

---

## Purpose

After 2–3 threads of active work, projects accumulate drift: skills get stale, claude.md
files fall behind, public_html collects zip files and temp pages, and decisions made in
threads never get written down anywhere permanent.

This skill runs a structured cleanup and produces:
1. **Updated files** — ready to deploy (skills, claude.md, client files, configs)
2. **A checklist** — anything found off in public_html
3. **A kickoff doc** — ready-to-paste brief for the next session

---

## When to Run

Jim triggers this with: "clean this project", "let\'s tidy up", "cleanup time", or similar.
Also run at the close of a major build cycle (new Spaces batch, full site launch, monthly
Ads/GSC review) even if Jim doesn\'t explicitly ask.

---

## Step 1 — Identify the Project

Confirm which project is being cleaned:

| Project | claude.md location | Skills to check | SSH user |
|---------|-------------------|-----------------|----------|
| 8th Street | ~/www/8thstreetsolutions.com/public_html/claude.md | foundation, porch, curb-appeal, google-ads, work-smarter | u3063-yznvsscqgtmo |
| Client site | ~/www/[domain]/public_html/claude.md on their SiteGround | foundation, porch, google-ads | client SSH user |
| PathAcross | ~/www/pathacross.com/public_html/claude.md on PathAcross server | pathacross, spaces, source-alignment | PathAcross SSH user |

**Skills directory (8th Street server):** `~/skills/user/[skill]/SKILL.md`

**Also check client file:** `clients/[client-id]/` on SiteGround — confirm it exists and
is current.

If unclear, ask: "Which project — 8th Street, a client site, or PathAcross?"

---

## Step 2 — Read Recent Threads

Search past conversations for work done since the last cleanup. Look for:

- Decisions that changed how something works
- New pages or features built
- Client or product changes
- Things marked "we\'ll update the skill later"
- Rules added in a thread but never written to the skill file
- Version numbers updated in code but not in claude.md

---

## Step 3 — Audit public_html

Two paths depending on what\'s available.

### Path A: SSH (primary for 8th Street)

```bash
# Check for junk
find ~/www/8thstreetsolutions.com/public_html/ -name "*.zip" -o -name "*.bak" -o -name "php_errorlog"
find ~/www/8thstreetsolutions.com/public_html/ -name "test*.php" -o -name "temp*.php" -o -name "*_old*"

# PHP lint
php -l ~/www/8thstreetsolutions.com/public_html/*.php

# Sitemap URL count
grep -c \'<loc>\' ~/www/8thstreetsolutions.com/public_html/sitemap.xml

# .htaccess
cat ~/www/8thstreetsolutions.com/public_html/.htaccess
```

### Path B: ZIP Upload (when Jim exports from File Manager)

```bash
# List contents
unzip -l /mnt/user-data/uploads/site.zip | head -80

# Extract and run full audit
unzip -q /mnt/user-data/uploads/site.zip -d /tmp/site

# Junk files
find /tmp/site -name "*.zip" -o -name "*.bak" -o -name "php_errorlog" | sort

# PHP lint
find /tmp/site -name "*.php" | sort | while read f; do
  result=$(php -l "$f" 2>&1)
  if echo "$result" | grep -q "error"; then echo "FAIL: $f"; fi
done
echo "Lint complete"

# Sitemap URL count
grep -c \'<loc>\' /tmp/site/public_html/sitemap.xml
```

**Flag anything found — do NOT delete without Jim confirming. Just list it.**

### Server Logs (review periodically)

When Jim provides log files:

```bash
# 500 errors by URL
for f in *.gz; do zcat "$f"; done | grep \' 500 \' | grep -oP '"GET [^"]*"' | sort | uniq -c | sort -rn | head -20

# 404 clusters
for f in *.gz; do zcat "$f"; done | grep \' 404 \' | grep -oP '"GET [^"]*"' | sort | uniq -c | sort -rn | head -20
```

What to look for:
- Trailing slash 500s — fix in .htaccess
- Googlebot hitting 500s daily — most damaging SEO issue
- GET /tel:xxxxxxxxxx as 404 — broken phone link
- 404 clusters on old URL patterns — add redirects for 10+ hits

---

## Step 4 — Compare claude.md to Reality

Read the current claude.md for the project.

Compare against what was learned in Step 2. Look for:
- Outdated product versions (api.php version, widget.js version, model name)
- File paths or build rules that have shifted
- Sections referencing work that\'s complete or abandoned
- Missing entries for things built but never documented
- Page inventory table — are new pages listed?

Draft an updated claude.md with only what changed. Do not rewrite sections that are
still accurate.

---

## Step 5 — Review Skills and Client Files

Based on thread review, identify which skills have drifted.

**Common drift patterns:**
- A new build pattern was used but the skill shows the old way
- A product changed (Porch version, model name, API version)
- A rule was added in a thread but never made it into the skill
- A live agent is running but the skill still says "planned"
- Known bugs discovered but not documented in the relevant skill

**For each drifted skill:**
- Draft specific targeted updates
- If skill needs full reconstruction — use skill-creation skill and run a dedicated pass
- Bump version number in header on every edit

**Also check the client file** (`clients/[client-id]/`):
- Performance data current?
- Open items still open or resolved?
- Change log up to date?
- Porch config drifted from live config?
- Google Ads conversion labels still accurate?

---

## Step 6 — Produce Outputs

### Output A: Updated Files
- `claude.md` — full updated file, ready to deploy via SSH
- Any skill updates — deployed via SCP + Python script, not just drafted
- Any client file updates — clearly marked with what changed

**Deployment pattern for skill updates:**
1. Write Python script locally
2. `scp -i ~/.ssh/id_ed25519 -P 18765 script.py u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com:/tmp/script.py`
3. `ssh ... "python3 /tmp/script.py"`
4. Verify: `head -6 ~/skills/user/[skill]/SKILL.md && wc -l ~/skills/user/[skill]/SKILL.md`

### Output B: public_html Checklist

```
PUBLIC_HTML AUDIT -- [Project] -- [Date]

[OK] Clean
  - [list things that looked fine]

[WARN] Needs Attention (review with Jim before acting)
  - [filename] -- [what it is / why it\'s flagged]

[DELETE] Safe to Delete (confirm before running)
  - [list only if clearly junk]

[REDIRECTS] Active redirects
  - [confirm active redirects match current site structure]

[SITEMAP] Sitemap status
  - [confirm all live pages are included / any missing]
```

### Output C: Next-Session Kickoff Doc

Not a summary — a ready-to-paste brief for the next thread:

```
NEXT SESSION KICKOFF -- [Project] -- [Date]

1. DEPLOY FIRST
   [Anything built this session not yet deployed]

2. GSC SUBMISSIONS
   [Complete URLs ready to paste, in priority order]
   [Reminder: 10 URL inspection/day limit. Jim handles.]

3. BUILD NEXT
   [What needs fresh GSC data or client input before proceeding]

4. OPEN QUESTIONS
   [Things waiting on Jim or client input]

5. FILES TO HAVE READY
   [What Jim should pull before next session starts]

6. SKILLS TO UPDATE
   [Any skill that drifted and needs a dedicated pass]
```

---

## Rules

- **Never delete files without Jim confirming** — flag them, don\'t touch them
- **Don\'t rewrite things that aren\'t broken** — targeted updates only
- **Source of truth hierarchy:** Live site + recent threads > old skill content
- **One project per run** — don\'t try to clean multiple projects in one pass
- **If a skill needs a major overhaul** — flag it and run skill-creation skill in a
  dedicated pass (this is what the March–April 2026 skill reconstruction pass was)
- **Cleanup is not the place for new builds** — note gaps in kickoff doc and move on
- **When cleanup surfaces a bigger issue** — assess scope. Quick fix → do it. Structural
  change → queue as Priority 1 in kickoff doc
- **Always deploy updates, don\'t just draft them** — cleanup output is live files on
  SiteGround, not artifacts in the conversation

---

*Clean Project — End-of-Cycle Discipline*
*8th Street Solutions / PathAcross*
*Run after every major work cycle. Skills live at ~/skills/user/ on SiteGround.*

---

## Closing Step — Session Debrief

Before ending any session, ask Claude to produce a session debrief. Route findings to the relevant skill before closing the thread.

**Prompt to use:**
"Give me a session debrief — what changed, what was decided, and what needs to go into which skill."

This is a required closing step, not optional. The debrief habit is only useful if findings land in skills before the thread closes.
