<!--
version: 2.0
date: 2026-04-01
source: Update — GSC filter note (lines 42+ = PathAcross), depression vertical parked,
        Space qualification criteria sharpened, build prompt template clarified,
        session start rule, "burden-and-depression.php" crisis reference preserved
-->

---
name: spaces
description: "Build PathAcross Spaces — keyword-first content hubs at PathAcross.com/[space]. Use when (1) identifying a new Space, (2) researching search terms for any content area, (3) building or expanding a Space, (4) writing or auditing content pages, (5) setting up Blue CTA patterns, (6) analyzing GSC data, (7) automating new page additions, (8) creating WisdomBase entries for a Space, (9) running monthly SEO/AISEO updates across Spaces. Always use this skill when Jim mentions new Space, search terms, GSC, add pages, WisdomBase Space entry, or build a /[topic] section. This is the primary forward build model — separate domains are legacy."
---

# PathAcross Spaces
## Build System — PathAcross.com/[space]

---

## The Mental Model

**Content is the entry. Blue is the point.**

A Space finds people searching for something specific, gives them genuine depth, and makes
it natural — not pushy, but unmistakable — that Blue can take them further.

Success is not impressions. Success is people clicking through to PathAcross.com and
starting a conversation with Blue.

**Each new Space builds the miniSLM corpus.** Volume of Spaces = volume of data = depth
of the future model. Aggressive Space expansion is the right strategy on every level —
traffic, bridge crossings, AND miniSLM training corpus simultaneously.

**Current Space status and open items in:** `references/system-state.md` in the
pathacross skill folder. Read that file before any new Space session.

---

## What This Skill Produces

- Output type: Complete Space folder (config.php + generate.php → all pages + sitemap.xml
  + wisdombase-entry.txt)
- Always includes: wisdombase-entry.txt flagged for Jim to paste manually via
  wb-capture.php; generate.php deleted after run; Space slug noted for spaces-registry.php
  if adding new Space
- Never includes: Auto-writing to WisdomBase (Jim only); new Space going live without
  Jim review
- Handoff format: Space build summary with slug, page count, sitemap entry count, and
  wisdombase-entry.txt contents as plain text block

---

## Currently Parked Verticals

**Depression vertical — DO NOT BUILD YET.**
Explicitly parked. Build after current Spaces show click movement and the PathAcross
platform proves the pattern. Not a timing issue — a sequencing discipline.

All other parked verticals listed in references/system-state.md.

---

## The Three Strategies

### Strategy 1 — Specific Situation
A defined life situation with real search volume. Not clinical categories — a specific
thing that happened to you. People are figuring something out. Pages answer real questions.
Blue meets the person when information runs out.

**Sweet spot:** 90–500 searches/month, specific situation, national
**Voice:** Honest, clear, depth-first — not clinical, not cheerful

### Strategy 2 — The Moment
What someone types from inside a bad moment — not a diagnosis, a felt experience. Lower
volume, almost no real competition, and the person typing is already looking for someone
to talk to. Highest Blue-fit of all three strategies.

**Sweet spot:** 30–200 searches/month — deliberately lower than Strategy 1
**Voice:** Recognition-first. Open with "you typed this because something is happening" —
not "here is what X is." The page meets the moment before it informs.

**Critical tone note:** Honest recognition — not hope, not despair, not manufactured urgency.
The person needs to feel seen, not sold to. For pages near crisis territory (Feeling Like
a Burden especially), include a gentle in-content acknowledgment — the 988 footer alone
is not enough. Reference: `burden-and-depression.php` — preserve this pattern if ever edited.

### Strategy 3 — Better Category
Clinical-adjacent topics chosen carefully — not broad labels but specific enough to own
the long tail where major health sites are weakest. Chosen against actual SERP landscape,
not category logic alone.

**Sweet spot:** 50–200 searches/month — focus on terms where Quora/Reddit rank, not WebMD
**Voice:** Bridges Strategy 1 and 2 — clinical grounding with recognition voice

---

## Architecture (Locked)

- Everything at `PathAcross.com/[space-slug]` — never a separate domain
- Domain authority compounds on PathAcross.com
- All GSC data in one Search Console property
- One universal template — one change touches every Space
- Start with 10 pages per Space. GSC drives expansion.
- National focus. State variation noted inline where relevant, never as separate pages.
- No Porch widget. No external images. No stock photography.

---

## File Structure

```
pathacross.com/spaces/
  _shared/
    header.php            ← Universal — NEVER modified per Space
    footer.php            ← Universal — NEVER modified per Space
    spaces.css            ← Universal — one file, all Spaces
    space-functions.php   ← Shared PHP helpers
    space-builder.php     ← Reusable page builder
    generate-template.php ← Starting point for every new Space generator
  [space-slug]/
    index.php             ← Hub page (written by generator)
    [topic].php           ← One per search term cluster
    config.php            ← PERMANENT — stays on server forever
    sitemap.xml           ← Written by generator automatically
    wisdombase-entry.txt  ← Temp — paste into wb-capture.php, then DELETE
  spaces-registry.php     ← Single source of truth for all Space slugs, labels, chips
  sitemap.xml             ← Sitemap index pointing to all Space sitemaps
  legal.php               ← Shared across all Spaces
```

**Universal files:** Edit once, updates every Space. Never create Space-specific versions.
**Content files:** Written by generator. Never hand-edited after generation.
**Post-run temp files:** generate.php and wisdombase-entry.txt — delete after use.

---

## Build Method — CC + SSH

**One Space per CC session. Always open with:**
```
Work completely autonomously. Do not pause, do not ask for confirmation at each step.
Make the best decision if you hit ambiguity and note it in the final summary.
Show me a summary when done.
```

**CC build steps:**
1. Read claude.md to orient
2. Create folder at `~/www/pathacross.com/public_html/spaces/[slug]/`
3. Write config.php and generate.php with full content (index + 9 topic pages)
4. Run generator — produces all pages + sitemap.xml + wisdombase-entry.txt
5. Delete generate.php after confirming all pages written
6. Leave wisdombase-entry.txt in place — flag in summary for Jim

**After CC completes:**
- Jim pastes wisdombase-entry.txt into wb-capture.php (source: Space), then deletes file
- Jim submits sitemap to GSC
- spaces-registry.php updated only when Jim confirms new Space is ready

**CC prompt template:**
```
Work completely autonomously. Show me a summary when done.

Read claude.md first to orient. Build the [Space Name] Space at
pathacross.com/spaces/[slug].
Strategy [1/2/3] — [Specific Situation / The Moment / Better Category]

Steps:
1. Create ~/www/pathacross.com/public_html/spaces/[slug]/
2. Write config.php, then generate.php with full content — index hub plus 9 topic pages
3. Run generator to produce all pages
4. Delete generate.php after confirming all pages written
5. Leave wisdombase-entry.txt in place — flag in summary

Search terms to target: [list 10 terms]
Voice: [strategy voice note]
What\'s underneath: [2-3 sentences on emotional reality]
Crisis numbers: [primary] primary, [secondary] secondary
```

---

## config.php Template

```php
<?php
/**
 * [SPACE NAME] — PathAcross Space
 * PERMANENT FILE — do not delete from server.
 */

require_once dirname(__DIR__) . \'/_shared/space-functions.php\';

$space_name    = \'[Space Display Name]\';
$space_slug    = \'[space-slug]\';
$space_url     = \'https://pathacross.com/spaces/\' . $space_slug;
$space_tagline = \'[One line — who this is for]\';
$space_arrival = \'[Who arrives here and what they are carrying]\';

$bridge_headline = \'[The limit — what information alone cannot do]\';
$bridge_body     = \'[What Blue offers that content cannot]\';
$blue_url        = \'https://pathacross.com/?from=\' . $space_slug;

$crisis_primary_label = \'988 — Crisis & Suicide Lifeline\';
$crisis_primary_num   = \'988\';
$crisis_backup_label  = \'[Space-appropriate line]\';
$crisis_backup_num    = \'[number]\';

$schema_type        = \'WebPage\';
$schema_description = \'[One sentence — what this Space provides]\';
$schema_abstract    = \'This information serves someone working through something real. Every person who arrives here deserves honest, clear information offered without judgment.\';

$og_title       = $space_name . \'| PathAcross\';
$og_description = $schema_description;
```

**Critical:** `require_once dirname(__DIR__) . \'/_shared/space-functions.php\';` must be
first line after docblock.

---

## generate.php Structure

```php
<?php
if (($_GET[\'token\'] ?? \'\') !== \'pa-monitor-2026\') { http_response_code(403); exit(\'Forbidden\'); }

require_once __DIR__ . \'/config.php\';
require_once dirname(__DIR__) . \'/_shared/space-builder.php\';

$pages = [
  \'index\' => [
    \'title\'            => \'[Space Name]\',
    \'meta_description\'  => "[Hub meta — double quotes always]",
    \'hero_h1\'           => \'[Hub headline]\',
    \'hero_lead\'         => \'[Who arrived and why this Space exists for them]\',
    \'quick_answer\'      => \'[2-3 sentence direct answer]\',
    \'recognition\'       => \'[Who this is for — the thing they haven\'t said out loud]\',
    \'faqs\' => [
      [\'q\' => \'[Question]\', \'a\' => \'[Answer]\'],
    ],
    \'topic_cards\' => [
      [\'slug\' => \'[topic-slug]\', \'h3\' => \'[Card heading]\', \'desc\' => \'[One sentence]\'],
    ],
  ],

  \'[topic-slug]\' => [
    \'title\'            => \'[Page title]\',
    \'meta_description\'  => "[150-160 chars — double quotes always]",
    \'hero_tag\'          => \'[Space Name]\',
    \'hero_h1\'           => \'[H1 — matches search intent]\',
    \'hero_lead\'         => \'[Recognition paragraph]\',
    \'quick_answer\'      => \'[2-3 sentences — specific, no hedging]\',
    \'sections\' => [
      [\'h2\' => \'[Section heading]\', \'content\' => \'<p>[HTML content]</p>\'],
    ],
    \'closing_recognition\' => \'[Final recognition — what becomes possible]\',
    \'faqs\' => [
      [\'q\' => \'[Question]\', \'a\' => \'[Answer]\'],
    ],
  ],
];

$wisdombase = [
  \'space_name\'       => \'[Space Name]\',
  \'who_arrives\'      => \'[Who is this person, what are they carrying]\',
  \'real_question\'    => \'[The emotional question under the search term]\',
  \'blue_should_know\' => \'[Key facts, language to use/avoid, emotional state on arrival]\',
  \'what_shifts\'      => \'[What actually moves people — what Blue can do that content cannot]\',
  \'misconceptions\'   => \'[What people get wrong that Blue should gently correct]\',
  \'bridge\'           => \'[The natural opening moment — when conversation can go deeper]\',
];

pa_run_generator($pages, $wisdombase);
```

### Critical rules — prevent 500 errors

**Double quotes for meta descriptions:**
```php
\'meta_description\' => "Here\'s what you need to know.",   // CORRECT
\'meta_description\' => \'Here\'s what you need to know.\',  // WRONG
```

**space-functions.php must be first line in config.php** (after docblock).

---

## Space Selection

### The Right Kind of Search Term
Not marketing keywords — search terms people actually type when dealing with something real.

**The SERP test:** Google the term. If page 1 is all major institutions with generic
content — find adjacent terms they have not covered deeply. If page 1 has Quora threads
and thin blogs — that is the gap.

**People Also Ask is a free keyword list.** Check PAA boxes for the primary term. These
are exactly what real people ask — 50–150 searches/month, low competition, directly what
someone in that moment would type.

### Space Qualification — All Four Required

| Criterion | Test |
|---|---|
| Search demand | Sufficient clusters at target volume for strategy |
| Blue fit | Would someone searching this want to think it through with something genuinely intelligent? |
| Rankable | Existing results are thin, generic, or not built for this person |
| Ethical | We can inform honestly without overstepping |

---

## Content Standards

**Every topic page:**
- 1,400–1,900 words
- Quick answer block (2–3 sentences — gets cited by AI systems)
- 4–6 h2 sections
- 4–6 FAQs with open format (not accordion)
- 8th grade reading level
- Paragraphs ≤4 sentences
- Closing recognition — what becomes possible now

**Moment Spaces:** Recognition-first voice. Page opens with the moment, not the definition.

**Crisis near-territory pages:** In-content acknowledgment on relevant pages — not just
footer 988. `burden-and-depression.php` is the reference example — preserve if ever edited.

---

## Adding a Space to the Registry

One entry in `spaces/spaces-registry.php`:

```php
\'[space-slug]\' => [
    \'label\'     => \'[label for opening message]\',
    \'chips\'     => ["[chip one — first person, specific]", "[chip two — first person, specific]"],
    \'type\'      => \'space\',
    \'space_url\' => \'https://pathacross.com/spaces/[space-slug]\',
],
```

Chips must be first-person phrases that sound like something a real person would say —
not topic labels.

**Registry timing:** One update when all planned Spaces are complete — not after each
individual Space.

---

## WisdomBase Entry Per Space

Generator writes `wisdombase-entry.txt` automatically after every build. Process:
1. CC leaves file in place and flags in summary
2. Jim pastes into `wb-capture.php?token=pa-monitor-2026` — Source type: Space
3. Jim deletes wisdombase-entry.txt from server

---

## GSC Feedback Loop

**In the 8th Street GSC scored report** (`~/gsc-agent/reports/scored_YYYY-MM-DD.txt`):
- Lines 42+: PathAcross data appears here
- PathAcross GSC analysis should be done via the PathAcross GSC property directly —
  not from the 8th Street cron output

**Per-Space GSC cycle:**
1. Export GSC Performance → Queries CSV (pathacross.com, filter by `/spaces/[space]/`)
2. Review:
   - Positions 4–20 with impressions → deepen content, improve FAQ
   - Queries with impressions, no matching page → build the page
   - Declining positions → identify cause, update content

---

## Design System

### Colors
```css
:root {
  --pa-dark:       #37415c;
  --pa-blue:       #6198c5;
  --pa-dark-10:    #eef1f6;
  --pa-blue-10:    #eef4fa;
  --pa-blue-20:    #ddeaf5;
  --pa-white:      #ffffff;
  --pa-text:       #2d3748;
  --pa-text-muted: #718096;
}
```

### Typography
```css
font-family: \'Playfair Display\', Georgia, serif;  /* h1, h2 */
font-family: \'Inter\', -apple-system, sans-serif;  /* body */
font-size: 18px;
line-height: 1.75;
max-width: 800px;   /* content column */
max-width: 1100px;  /* container */
```

### Layout Rules
- Mobile-first. Every decision starts from 375px.
- Minimum touch target: 44px
- Textarea font-size: 16px minimum (prevents iOS zoom)
- Header: locked — logo links to Space hub, nav is Home + Talk to Blue pill only
- Footer: three sections, centered, font-size 0.9rem

---

## Agent Mode

**What runs dark:** File generation, page builds, sitemap.xml updates, generate.php deletion.
**Human touchpoints:** wisdombase-entry.txt is never auto-written — Jim pastes it manually.
No Space goes live without Jim review. spaces-registry.php updated only when Jim confirms.

---

*PathAcross Spaces — Content is the entry. Blue is the point.*
*Methodology only. Current Space status in pathacross/references/system-state.md*
*Scales to any number of Spaces without skill changes.*
