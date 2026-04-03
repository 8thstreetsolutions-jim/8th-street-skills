<!--
version: 1.1
date: 2026-04-02
source: Added version header for manifest compatibility
v1.1: Added output contract
-->
---
name: foundation-cd-dui
description: "Build demo and full Foundation sites for criminal defense and DUI law firms. Use when (1) building or updating any law firm website for a CD/DUI firm, (2) creating a demo page for prospecting, (3) writing a Porch config for a law firm, (4) planning content for DUI or criminal defense pages, (5) sourcing attorney bios or firm credentials. Active clients: Andy Sotiropoulos (DUI, Illinois) and Heart of Illinois / Maureen (CD+DUI, Illinois). Always use this skill when Jim mentions a law firm demo, CD/DUI site build, law firm Porch config, or either active client."
---

# Foundation CD/DUI
## Criminal Defense & DUI Law Firms

---


## What This Skill Produces
- Output type: Built or updated Foundation site for a CD/DUI law firm (PHP files deployed to SiteGround)
- Always includes: State-specific DMV deadline verified and present on homepage, DUI page, and Porch KB; reading level check (target 8th grade); change logged in `clients/[client-id]/change-log.md`
- Never includes: DMV deadlines assumed without verification; content published without Jim review; generic (non-vertical) copy patterns
- Handoff format: Live page URLs + Porch KB entries (if written) + any client info still needed

---

## Philosophy

Every CD/DUI prospect is scared and searching at 2am. The site's job is to answer every question they'd ask ChatGPT — but better — and then make calling feel like the obvious next step.

The firm's differentiators almost always fall into two buckets:
1. **Credentials** — where they trained, what they've won, recognitions
2. **Accessibility** — they pick up, they explain things, they're not a factory

Lead with both. Never make them hunt for the phone number.

For DUI specifically: the DMV hearing deadline is the single most important urgency driver. It belongs on the homepage, the DUI page, and in the Porch KB. **Always verify the deadline for the specific state** — it varies (Oklahoma = 15 days, Colorado = 7 days, Illinois = 90 days). Never assume.

---

## Reading Level Standard

Target **8th grade reading level** for all public-facing content.
- Flesch Reading Ease score: 60–70
- Average sentence length: 15 words or fewer
- Average syllables per word: 1.4 or fewer

**Why this matters:** CD/DUI clients are scared, stressed, and often reading on a phone at 2am. Long sentences with legal vocabulary lose them. Short, direct sentences build trust.

**Reading level check script:**
```python
import re

def check_reading_level(filepath):
    with open(filepath, 'r') as f:
        content = f.read()
    content = re.sub(r'', '', content, flags=re.DOTALL)
    content = re.sub(r']+>', ' ', content)
    content = re.sub(r'\s+', ' ', content).strip()
    sentences = [s.strip() for s in re.split(r'[.!?]+', content) if len(s.strip()) > 20]

    def syllables(word):
        word = word.lower().strip(".,!?;:'\"")
        if len(word) <= 3: return 1
        vowels = 'aeiouy'
        count, prev = 0, False
        for c in word:
            v = c in vowels
            if v and not prev: count += 1
            prev = v
        if word.endswith('e'): count -= 1
        return max(1, count)

    words = total = syls = 0
    for s in sentences[:30]:
        w = s.split()
        if not w: continue
        total += 1; words += len(w); syls += sum(syllables(x) for x in w)

    if total and words:
        fk = 206.835 - (1.015 * words/total) - (84.6 * syls/words)
        print(f"Flesch: {fk:.1f} | Avg sentence: {words/total:.1f} words | Grade: {'8th' if fk >= 60 else 'College'}")
```

Run on every content page before delivery. If score is below 60, shorten FAQ answers and intro paragraphs first — those move the score the most. Statute quote sections will always score low; that is acceptable. Target the surrounding prose.

---

## Active Clients

| Client | Site | Focus | State |
|--------|------|-------|-------|
| Andy Sotiropoulos | illinois-dui-defender.com | DUI only | Illinois |
| Heart of Illinois / Maureen | heartofillinoiscriminaldefense.com | CD + DUI | Illinois |

**Note on Maureen:** Heart of Illinois also has a sponsored public resource site at `peoriacriminaldefense.com`. That site uses the **foundation-business Track 3** model — not this skill. Keep them separate. The law firm site (Heart) is built and maintained here. The resource site (PCDR) follows Track 3 patterns. Site is live, GSC active, SSH access confirmed as of April 2026.

For Google Ads on these accounts → **google-ads** skill.
For GSC/organic growth → **Curb Appeal** skill.

---

## Session Kickoff

Jim provides at the start of every demo build:
1. **4–5 URLs from the firm's existing site** — homepage, about, DUI, CD, contact. If no site, skip to intake template.
2. **Target city for the demo**
3. **Any specific angle** — e.g. "former DA," "only firm in county," "bilingual"

Claude fetches each URL with `web_fetch`, extracts intake data automatically, fills template before building anything.

**Site Fetch Process:**
1. `web_fetch` each URL one at a time
2. Extract: firm name, attorney names + credentials, practice areas, phone, address, counties, reviews, color/brand signals
3. Note what's missing or weak — becomes demo talking points for John
4. Flag anything unusual
5. Confirm filled intake template with Jim before writing any code

---

## Intake Template
```
FIRM BASICS
Firm name:
Address (full):
Phone: / Email: / Website:
States licensed: / Counties served:
Free consultations? Y/N

ATTORNEYS (repeat per attorney)
Full name: / Bar admission:
Law school + year:
Former positions (DA, public defender, Supreme Court, BigLaw):
Notable results / Recognitions:
Languages:
Short bio:

PRACTICE AREAS
[ ] Criminal Defense  [ ] DUI/DWAI/OWI  [ ] Drug Charges
[ ] Domestic Violence  [ ] Assault/Battery  [ ] Theft
[ ] Federal Crimes  [ ] Expungement  [ ] Other: ___

GEOGRAPHY
Primary courthouse(s):
Other counties/jurisdictions:

DIFFERENTIATORS
What makes them different? / Answer their own phone?
Known for anything specific?

REVIEWS (3-5 from Google/Avvo)

BRANDING
Logo file or initials: / Primary color:

ATTORNEY PHOTOS
Number with photos: / Files or source URLs:
Upload to: assets/[firstname-lastname].jpg

NOTES
State-specific DUI DMV deadline: (verify — never assume)
Full pages vs. stubs?
```

---

## Photo Processing

All attorney photos processed to matching dimensions before building.
```python
from PIL import Image
TARGET_W, TARGET_H = 400, 500

# Landscape — crop center portrait region
img = Image.open('photo.webp')
w, h = img.size
crop_w = int(w * 0.55)
crop_x = (w - crop_w) // 2
cropped = img.crop((crop_x, 0, crop_x + crop_w, h))
final = cropped.resize((TARGET_W, TARGET_H), Image.LANCZOS)
final.save('assets/attorney-name.jpg', 'JPEG', quality=90)

# Portrait — crop top for head+shoulders
img = Image.open('photo.webp')
w, h = img.size
crop_bottom = int(h * 0.62)
cropped = img.crop((0, 0, w, crop_bottom))
final = cropped.resize((TARGET_W, TARGET_H), Image.LANCZOS)
final.save('assets/attorney-name.jpg', 'JPEG', quality=90)
```

All photos same dimensions (400x500px). Always view crop before building. Save as jpg. Never hotlink.

---

## Page Map

### Full-Content Pages
- `index.php` — Hero, urgency bar, practice area grid, why us, attorneys, reviews, counties, CTA
- `about.php` — Attorney profiles
- `dui-defense.php` — DMV deadline warning, two-proceedings, local court, FAQ + schema
- `criminal-defense.php` — Lead with fear, process, local court, FAQ + schema
- `contact.php` — Form + office info + map embed
- `faq.php` — 13+ questions, FAQPage schema

### Stub Pages
- `expungements.php` — worth building fully (high search intent)
- `drug-charges.php` / `domestic-violence.php` / `federal-crimes.php` / `juvenile.php`
- `personal-injury.php` (if applicable)
- `counties.php` — service area hub

**Stub pattern:** Real 2-3 sentence intro + dashed note box + CTA.

### Out-of-Scope Practice Areas
If firm also does family law, bankruptcy, PI — build CD/DUI pages fully, add stubs for the rest, flag to Jim: "This firm does [X] — needs the **foundation-law-firms** skill."

---

## File Structure
```
demo/[client-slug]/
  index.php / about.php / practice-areas.php
  dui-defense.php / criminal-defense.php
  expungements.php / [other stubs].php
  counties.php / faq.php / contact.php / disclaimer.php
  [client-slug]-porch-config.json
  includes/header.php / includes/footer.php
  assets/styles.css / favicon.svg / [attorney].jpg
```

---

## Required Files Checklist

Every build must include these before delivery:
- [ ] `sitemap.xml` — all pages with priorities
- [ ] `robots.txt` — Allow all, Sitemap pointer
- [ ] `.htaccess` — ErrorDocument 404, HTTPS force, security headers, asset caching
- [ ] `404.php` — full navigation, no giant error number, links to all key pages
- [ ] `favicon.svg` — firm initials, accent color on primary background

---

## Design Standards

### Colors — Always decide before writing any code
- **Navy + gold** (default, new demos): Navy `#1a2744`, Gold `#c9a227`
- **Charcoal + red** (matching existing brand): Charcoal `#202020`, Red `#c52c35`
- Never carry colors from a previous demo

### CSS Variables (navy/gold standard)
```css
:root {
  --primary: #1a2744;       --primary-dark: #111b30;
  --accent: #c9a227;        --accent-dark: #a8861f;
  --text: #1e1e1e;          --gray-text: #5a5a5a;
  --gray-light: #f4f3f1;    --gray-mid: #ddd;
  --white: #fff;
  --font-display: 'Playfair Display', Georgia, serif;
  --font-body: 'Source Serif 4', Georgia, serif;
  --max-width: 1100px;      --pad: 24px;
}
```

### Hero/Header Text — Critical Rule
When hero or page-header background is mid-tone (grey, brand color, not near-black) — **force ALL text to white:** eyebrow, h1, sub, stats, breadcrumb. Default: force white unless background is near-black.

### Light Palette Pattern
For resort/outdoor markets or existing light brands:
```css
:root {
  --primary: #d93527;
  --header-bg: #f0eeec;   --footer-bg: #f0eeec;
  --section-alt: #f5f3f1;
}
```
Nav links use `var(--text)` not white.

### Attorney Layout
- Single attorney: full `.attorney-profile` (large photo, full bio)
- 2+ attorneys: `.attorneys-grid` — side-by-side cards (160x200px photos)

---

## Content Standards

### Homepage Pattern
Hero → Urgency bar (DMV deadline) → Practice areas grid → Why Us → Attorney section → Reviews → Counties → CTA block

### DUI Page
Warning box with state-specific deadline → Two-proceedings explanation → Local courthouse → 8+ FAQ items + FAQPage schema

### Criminal Defense Page
Lead with the fear → Process (arrest → arraignment → discovery → plea or trial) → Local courthouse → FAQ + schema

### FAQ Page
13+ questions, all open (never accordion). Cover: DUI deadline, APC, felony vs misdemeanor, arraignment, should I talk to police, do I need a lawyer if guilty, expungement eligibility, cost, timeline.

### Voice
Fight messaging outperforms comfort messaging. Never Mr./Ms. — always first + last name. No emojis. No accordion FAQs.

---

## Schema
```json
{
  "@context": "https://schema.org",
  "@graph": [
    { "@type": "LegalService", "@id": "https://[domain]/#firm" },
    { "@type": "Attorney", "worksFor": { "@id": "https://[domain]/#firm" } }
  ]
}
```
FAQPage schema on faq.php, dui-defense.php, criminal-defense.php.

---

## Porch Config Standard

**Never give legal advice or case opinions.**
```json
{
  "clientId": "[slug]",
  "businessName": "[Firm Name]",
  "topic": "criminal defense and DUI law firm",
  "phone": "[phone]",
  "prompt": "You are the virtual assistant for [Firm Name]. Be brief — 2-3 sentences max. Help visitors understand what the firm handles and how to reach [attorney first name]. Never give legal advice or opinions on their specific case. Direct to call for a free consultation.",
  "kb": "[Firm Name] — [address]. Phone: [phone]. Attorneys: [names, credentials]. Practice areas: [list]. Counties: [list]. Free consultations. DUI clients have only [X] days from arrest to request a DMV hearing.",
  "exitGreeting": "Before you go — [attorney] offers free consultations. Call [phone] anytime.",
  "exitQuickReplies": ["Call now", "What do you handle?", "Where are you located?"]
}
```

Footer embed:
```html
<script src="https://8thstreetsolutions.com/porch/porch.js"></script>
<div id="porch-widget" data-client="[slug]"></div>
```

---

## Research Workflow

1. Fetch existing site (Jim provides 4-5 URLs)
2. Avvo — ratings, reviews, bio
3. Martindale — peer reviews
4. LinkedIn — education, former firms
5. Google reviews — pull 3-5 verbatim
6. Competitor scan — 2-3 local firms
7. Courthouse — identify by name
8. State DMV deadline — verify, never assume

---

## Demo Delivery Checklist

- [ ] Color palette decided before any code written
- [ ] Attorney photos processed to matching 400x500px JPGs
- [ ] Correct include paths (`include 'includes/header.php'` from root)
- [ ] Photos in `assets/[name].jpg` — not hotlinked
- [ ] Favicon `assets/favicon.svg` (initials, accent color)
- [ ] Phone and address verified against firm's own site
- [ ] All text white on mid-tone hero/header backgrounds
- [ ] No demo banner, no emojis
- [ ] Attorney names used in full throughout
- [ ] DUI deadline state-verified
- [ ] Forms light background, accent focus border
- [ ] Mobile sticky CTA bar present
- [ ] Porch widget in footer.php
- [ ] Porch config in zip as `[slug]-porch-config.json`
- [ ] sitemap.xml present and complete
- [ ] robots.txt present
- [ ] .htaccess present (404 + HTTPS)
- [ ] 404.php present with full navigation
- [ ] Reading level checked — target Flesch 60-70

---

## Common Mistakes

Carrying colors from previous demo · Hotlinking photos · Mismatched photo sizes · Wrong DMV deadline · Accent text on mid-tone hero · Dark forms · Mr./Ms. instead of full name · Accordion FAQs · Legal advice in Porch · Wrong address/phone from directories · Missing sitemap/robots/htaccess · 404 page missing · Reading level not checked

---

## Build Process — Anti-Stall Rules

Build to disk, zip at end. Never build file-by-file as artifacts.
```
1. Confirm intake complete
2. Decide color palette — before any code
3. Process photos — confirm crop before building
4. Build all files to /home/claude/[slug]/
5. Zip from INSIDE the directory
6. Present zip
```
```bash
cd /home/claude/[slug]
zip -r ../[slug].zip .
```

---

## Reference Demos

### Doug Smith (March 2026)
`8thstreetsolutions.com/demo/dougsmithlaw/` · Navy/Gold · Oklahoma (15-day deadline) · 1 attorney

### Fassio Law (March 2026)
`8thstreetsolutions.com/demo/fassiolaw/` · Charcoal/Red · Oklahoma (15-day) · 2 attorneys · Grey hero → all text white · **Canonical CSS/architecture reference**

### Melnick Timmerman (March 2026)
`8thstreetsolutions.com/demo/melnicktimmerman/` · Red/Warm Grey · Colorado (7-day deadline) · 2 attorneys · First light palette build

---

## Cross-References
- Other law firm practice areas → **foundation-law-firms**
- General businesses → **foundation-business**
- Public resource/sponsored sites → **foundation-business Track 3**
- Google Ads → **google-ads**
- GSC/organic → **Curb Appeal**

---

*Foundation CD/DUI · Active clients: Andy Sotiropoulos, Heart of Illinois*
*Updated April 2026 — peoria site status added, Track 3 cross-reference confirmed*

---
*Always follow the Session Close Routine in the work-smarter skill at the end of every session.*
