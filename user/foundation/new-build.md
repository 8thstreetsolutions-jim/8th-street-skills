# Foundation — New Build Process
## Site Types, Execution Rules, Content Standards

---

## Execution Order

For any new Foundation build:
1. Read `clients/[client-id]/context.md`
2. Read `standards.md` — file structure, config, PHP rules
3. Read `verticals.md` — for this client\'s business type
4. Read `schema.md` — for correct schema type and patterns
5. Build. Do not ask questions the skill already answers.
6. `php -l` every file. `touch .htaccess` after deploy.
7. Log all changes in `change-log.md`.

---

## Site Types

### Main Site
The client\'s primary web presence.

**Page count target:** 40–60 pages at build. Grows with weekly Curb Appeal cycle.

**Page architecture:**
- Homepage
- Core service pages (one per main offering)
- Location pages (one per city/area served)
- About / team
- FAQ
- Contact
- Disclaimer / Privacy

**Content philosophy:** Answer every question a prospect would ask Google or an AI before
calling. The site\'s job is to make the phone call feel like the obvious next step.

---

### Second Home
A parallel Foundation site running alongside a client\'s existing website.

**When to use:**
- Client has WordPress or another CMS they\'re not ready to leave
- Client wants proof before committing to a switchover
- Client wants a second domain targeting a different audience or keyword cluster

**Two models:**
- Model A: Parallel marketing site on new domain
- Model B: Community resource site where 8th Street owns domain/traffic, professional
  sponsors it (see Resource Site below)

**Domain strategy (in order of preference):**
1. Unused domain the client already owns
2. New geo+keyword domain (e.g. `chicagoestateattorney.com`)
3. Subdomain of existing (last resort)

**The proof conversation:** GSC on the Second Home domain from day one. Monthly comparison
vs existing site. When our numbers exceed theirs, switchover conversation is easy.

**Typical switchover timeline:** 90–180 days for meaningful organic comparison.

**Switchover checklist:**
- [ ] 301 redirect map from all old URLs to new Foundation URLs
- [ ] GSC: add new property, link to old
- [ ] Update Google Business Profile URL
- [ ] Update all directory listings
- [ ] Update Porch config if domain changes
- [ ] Keep old site accessible 30–60 days as backup

---

### Resource Site (Community Resource Model)
A public-facing information resource. Professional sponsors it — footer mention only.
8th Street owns the domain and builds long-term traffic asset.

**Reference build:** `peoriacriminaldefense.com` — March 2026. Peoria model = the template.

**When to use:**
- Professional wants authority and traffic without rebuilding their main site
- A public information gap exists in their market
- 8th Street owns the domain

**Design principles:**
- White background, clean, civic feel — not advertising
- Logo: wordmark + tagline only ("A free guide for [topic] in [area]")
- Voice: factual, plain language — never urgency or fear messaging
- No Porch on the resource site itself

**Footer pattern:**
- Col 1: Local resources (courthouses, agencies, relevant public bodies)
- Col 2: Topic/page navigation
- Col 3: "Sponsored by" + firm name + 1–2 sentence description + link + disclaimer

**Page architecture:**
- Homepage: hero + stat band + topic grid + understand section + about band + FAQ
- Hub pages: short intro + cards linking to sub-pages (~400 words)
- Topic pages: 1,200+ words, minimum 2 statute/fact boxes, 6–8 open FAQs

**8th Street owns:** Domain, hosting, GSC property, all traffic and ranking equity.
**Sponsor gets:** Footer mention + link + traffic from resource site visitors.

---

## Content Depth Over Breadth

One topic covered thoroughly outperforms ten topics covered superficially.

**The three questions — apply to every page hero:**
1. **What is this?** — Plain English, no jargon, first block
2. **Is this for me?** — Name the specific person and situation
3. **Can I trust it?** — Trust signals placed next to the claims that need them

Generic messaging converts no one. Specific messaging converts.

---

## Hub Index Pattern

For any site with a content hub (e.g., `/legal-marketing/`, `/cities/`, `/porch/`):

**Stable insertion anchor in hub `index.php`:**
```html
<!-- NEW_CARDS_INSERT_BEFORE -->
```
Insert new `<a>` card blocks immediately before this comment.

**When adding a page to a hub, always:**
1. Add the PHP file in the hub subdirectory
2. Add the card to `index.php` using the insertion anchor
3. Update the deck line count in `index.php`
4. Add URL to `sitemap.xml` (priority 0.9 primary, 0.8 secondary)
5. Update the page inventory table in `claude.md`
