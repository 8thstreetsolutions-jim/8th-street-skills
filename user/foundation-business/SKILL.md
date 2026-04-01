---
name: foundation-business
description: "Build demo and full Foundation sites for non-law-firm businesses. Three tracks: (1) $500 small business demos for HVAC, plumber, electrician, roofer, landscaper, auto repair; (2) larger business clients with full marketing presence; (3) public resource / sponsored information sites built for SEO and AISEO authority, sponsored by a professional firm. Use when building or updating any business website that is NOT a law firm. Always use this skill when Jim mentions a business demo, the $500 pipeline, or any non-law-firm client site. Reference build for Track 3: peoriacriminaldefense.com."
---

# Foundation Business
## Non-Law-Firm Businesses

---

## Three Tracks

### Track 1: $500 Demo Pipeline (Small Business)
Fast demo builds for solo operators and small shops with no/weak websites. Target categories: HVAC, Plumber, Electrician, Roofer, Landscaper, Auto Repair, Pest Control, Dentist.

**Pipeline:** Google Places API → score weak sites → build demo → SMS owner → $500 close → go live in 24 hours.

**Prospect signals (strongest first):**
1. No website at all
2. Copyright 2015-2018 in footer
3. Not mobile responsive
4. No SSL
5. No visible phone/CTA

**Demo template lives at:** `demo.8thstreetsolutions.com/[businessslug]/`

**Outreach message:**
> Hi, I'm John with 8th Street Solutions. I built a new website for [Business Name] — take a look: demo.8thstreetsolutions.com/slug. $500 to go live. Interested?

**After close:**
> "Great — send me a few photos of your work and describe your services. Live in 24 hours."

**Config variables per client:**
- business_name, category, city, state, address, phone, email, domain
- primary_color (from logo or category default)
- hero_image_id (Unsplash ID by category — see prospecting skill)
- service_1/2/3 (name, desc, icon)
- owner_name, about_blurb, testimonials x3, credentials x3

---

### Track 2: Larger Business Clients
Full Foundation sites for established businesses that want a real marketing presence. Includes Porch, Google Ads, Curb Appeal. More complex builds — custom scope per client.

---

### Track 3: Public Resource / Sponsored Information Site

A public-facing information resource site sponsored by a professional firm (law firm, medical practice, financial advisor, etc.). The site serves the public with factual, deeply researched content. The sponsoring firm appears in the footer only — not as the headline. This model builds SEO and AISEO authority over time while providing genuine community value.

**When to use Track 3:**
- A professional firm has domain authority or a domain name they are not using
- The firm is well-established and wants to be seen as a community resource
- The target audience needs reliable plain-language information (legal, medical, financial)
- The goal is organic search authority, not direct advertising

**Reference build:** `peoriacriminaldefense.com` — March 2026
- Sponsored by Heart of Illinois Criminal Defense (Maureen, Peoria area)
- 19 pages: homepage, hub pages, 12 charge-specific pages, process pages, FAQ
- Colors: slate blue `#2C4A6E` + bronze `#B5762A`
- Built in one session here + CC for bulk pages
- Site is live, GSC active, SSH access confirmed as of April 2026

**Track 3 Design Principles:**
- White background, clean, civic feel — not law firm advertising
- Logo: wordmark only + tagline describing the resource
- Tagline format: "A free guide for those facing [topic] in [area]"
- Voice: factual, plain language, informational — never fight/urgency messaging
- Target reading level: **8th grade** (Flesch score 60–70). Check with Python script below.
- No Porch widget on the resource site itself — sponsoring firm handles that on their own site
- No mobile sticky CTA bar — this is an information resource, not a conversion site

**Track 3 Page Architecture:**
- Homepage: hero + stat band + topic grid + understand section + about band + FAQ
- Hub pages: short intro + cards linking to sub-pages (~400 words)
- Topic/charge pages: 1,200+ words, minimum 2 statute/fact boxes, 6-8 open FAQs
- Nav: 6 items max — Home, [Topic Hub], [Key page], [Key page], About, FAQ
- Footer: 3 columns — local resources col 1, topic links col 2, "Sponsored by" col 3

**Track 3 Content Standards:**
- Every topic page cites actual statutes or authoritative sources
- Statute box format: cite → brief quote → plain English explanation
- Local specificity on every page: courthouse names, local process, county context
- FAQPage JSON-LD schema on every page
- Article schema on content pages
- sitemap.xml, robots.txt, .htaccess (404 routing + HTTPS force) required
- 404 page: no giant error number — full navigation grid so users find content

**Track 3 SEO / AISEO Layer:**
- Specific citations ("625 ILCS 5/11-501") not vague references ("Illinois law says")
- Questions phrased as someone would ask an AI, answers as complete standalone paragraphs
- Depth over breadth — one topic thoroughly beats ten topics superficially
- FAQPage schema is the technical layer; content quality determines AI citation
- Submit sitemap to GSC after launch

**Track 3 Footer — Sponsored By Pattern:**
```
Col 1: Local resources (courthouses, legal aid, relevant public agencies)
Col 2: Topic/page navigation links
Col 3: "Sponsored by" tag + firm name + 1-2 sentence description + link
       Disclaimer: "This site is an independent public resource. Not legal/medical advice."
```

**Track 3 Replication Checklist (for new clients):**
- [ ] Confirm domain name owned or available
- [ ] Confirm domain in same SiteGround account (easiest deployment)
- [ ] Identify 10-15 topic areas to cover
- [ ] Decide colors — civic/trustworthy palette, distinct from sponsor's main site
- [ ] Logo: wordmark + tagline only, no icon needed
- [ ] Build homepage + 1 sample interior page here first for approval
- [ ] Hand off to CC with brief for remaining pages
- [ ] CC brief must include: statute references per page, local courthouse details, reading level target, autonomous build instruction
- [ ] After CC delivery: add 404/sitemap/robots/htaccess/favicon if missing (expungement is a content page, not a utility file)
- [ ] Upload to SiteGround, submit sitemap to GSC

**Potential Track 3 verticals beyond law:**
- Medical practice sponsoring a local health information resource
- Financial advisor sponsoring a personal finance guide for a metro area
- Accountant sponsoring a small business tax resource
- Real estate attorney sponsoring a homebuyer rights guide
- Any licensed professional where public education builds authority

---

## Reading Level Standard (All Tracks)

Target **8th grade reading level** for all public-facing content.
- Flesch Reading Ease score: 60–70
- Average sentence length: 15 words or fewer
- Average syllables per word: 1.4 or fewer
- Use plain language, short sentences, active voice

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

Run this on every content page before delivery. If score is below 60, shorten sentences in the FAQ answers and intro paragraphs first — those move the score the most.

---

## What Carries Over from Foundation CD/DUI

Same build mechanics: file structure, CSS variable pattern, build to disk + zip, header/footer includes pattern.

**What's different from law firm sites:**
- No legal/medical advice restrictions (Track 1 and 2)
- Urgency drivers vary by category (seasonal for HVAC, emergency for plumber)
- Emotional framing is practical not fear-based
- Pricing is often displayed directly (unlike law firms)
- Reviews from Google/Yelp/Angi rather than Avvo/Martindale

---

## Content Patterns by Category

### HVAC
- Urgency: seasonal (summer AC, winter heat), emergency breakdowns
- Differentiators: same-day service, licensed + insured, local family-owned
- Key content: what services, service area, financing, emergency line

### Plumber
- Urgency: emergency (burst pipe, no water) or planned (remodel)
- Differentiators: 24/7, licensed, upfront pricing

### Electrician
- Urgency: safety (panel issues, outlets), permits, remodel
- Differentiators: licensed, code-compliant, local

### Roofer
- Urgency: storm damage, leak, insurance claim
- Differentiators: insurance work experience, warranty, local

### Home Services (General)
- Local trust signals primary — years in business, reviews, photos of actual work
- Pricing: ranges are fine, exact quotes require visit
- CTA: always free estimate

---

## Porch Config Notes

Business Porch configs are less constrained than law firm configs:
- Can discuss pricing ranges and general service questions
- Can quote ballpark service call costs
- Can describe the process of getting an estimate
- Still: direct to owner for specific quotes, don't commit to pricing

---

## Hero Image IDs (Unsplash — for demo builds)

| Category | Unsplash ID |
|----------|-------------|
| HVAC | photo-1621905251189-08b45d6a269e |
| Plumber | photo-1558618666-fcd25c85cd64 |
| Electrician | photo-1621905252507-b35492cc74b4 |
| Roofer | photo-1504307651254-35680f356dfd |
| Landscaper | photo-1416879595882-3373a0480b5b |
| Auto Repair | photo-1486262715619-67b85e0b08d3 |
| Dentist | photo-1588776814546-daab30f310ce |
| Law Firm | photo-1589829545856-d10d557cf95f |

---

## Prospecting Tools

- `places-proxy.php` on SiteGround — Google Places API server-side
  - Endpoint: `https://demo.8thstreetsolutions.com/places-proxy.php`
  - Actions: `?action=search&query=HVAC+Denver` and `?action=details&place_id=XXX`
- Prospector artifact (`prospector-v3.jsx`) — search by category + city, score sites, generate config + outreach text
- Twilio SMS — `twilio-proxy.php` on SiteGround (Account SID, Auth Token needed)

**Conversion math (validate with data):**
50 texts/week → 10% response → 60% close = 3 sites/week = $1,500/week

---

## When to Flag to Jim

- Business client with specific industry regulations or professional liability concerns
- Healthcare businesses — treat like medical, not general business
- Track 3 builds where the sponsoring firm's professional license could be implicated by site content

---

*Foundation Business — Non-Law-Firm Businesses*
*Three tracks: $500 demos + larger business clients + public resource/sponsored sites*
*Track 3 reference build: peoriacriminaldefense.com — March 2026*
*Updated April 2026 — mobile CTA removed from Track 3 standard, replication checklist corrected*

---
*Always follow the Session Close Routine in the work-smarter skill at the end of every session.*
