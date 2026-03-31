# Foundation — Schema Standards
## Structured Data, AI Visibility, p.section-deck

---

## Schema Standards by Page Type

Every page outputs the global `LocalBusiness` / `LegalService` schema via header.
Page-level schema is additive.

| Page type | Schema type |
|-----------|-------------|
| Homepage | WebSite (additional) |
| About/Company | AboutPage |
| Service/How-it-works | Service |
| Contact | ContactPage |
| FAQ | FAQPage (set `$faq_schema` before header, output after) |
| Article/Case study | Article |
| Data/Content page | WebPage |
| Section index / hub | CollectionPage |
| Legal/404 | None (global schema from header is sufficient) |

---

## Page-Level Schema Pattern

```php
<!-- Schema: [Type] -->
<script type="application/ld+json">
<?php echo json_encode([
    "@context" => "https://schema.org",
    "@type"    => "WebPage",
    "@id"      => $site_url . "/slug#webpage",
    "url"      => $site_url . "/slug",
    "name"     => $page_title,
    "description" => $page_description,
    "isPartOf" => ["@id" => $site_url . "/#website"]
], JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES); ?>
</script>
```

Use `$site_url`, `$business_name`, `$page_title`, `$page_description` — never hardcode.

**Critical rule:** `$schemaMarkup` must be a raw JSON string, NOT a PHP array. The header
include outputs it directly. Do not pass a PHP array to `json_encode()` — causes malformed
JSON in the output.

---

## Entity Markup Priorities (2026)

Entity markup is the highest-leverage schema work in 2026.

Every site requires:
- Primary schema object with `@id` on the entity
- `sameAs` array (GBP, Avvo, LinkedIn, state bar, Justia — confirm URLs before adding)
- `knowsAbout` array — granular, specific to practice areas and geographies
- PostalAddress block on every schema object
- FAQPage on every page with FAQ content
- BreadcrumbList on every interior page
- JSON-LD only — no microdata

**Schema type by vertical:**
- Law firms: LegalService (not Attorney alone)
- Medical: MedicalBusiness + Physician for individual practitioners
- Professional services: ProfessionalService
- Local/trade: LocalBusiness with service area

---

## Homepage hasOfferCatalog Pattern

```php
"hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Services",
    "itemListElement": [
        {
            "@type": "Offer",
            "itemOffered": {
                "@type": "Service",
                "name": "[Service Name]",
                "description": "[One sentence]"
            }
        }
    ]
}
```

---

## FAQPage Schema Pattern

Set `$faq_schema` before header include, output after:

```php
$faq_schema = json_encode([
    "@context" => "https://schema.org",
    "@type" => "FAQPage",
    "mainEntity" => [
        [
            "@type" => "Question",
            "name" => "[Question text]",
            "acceptedAnswer" => [
                "@type" => "Answer",
                "text" => "[Answer text]"
            ]
        ]
    ]
], JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES);
```

10 Question objects minimum per page. Matching open h3/p pairs in HTML body — not
accordion. FAQ answers must stand alone (no "as mentioned above").

---

## p.section-deck Pattern

Every H2 gets a `<p class="section-deck">` immediately below it.

**CSS (in assets/styles.css):**
```css
p.section-deck {
    font-size: 1rem;
    font-weight: 700 !important;
    color: #475569;
    margin-top: 0.4rem;
    margin-bottom: 0.4rem;
    line-height: 1.5;
    display: block;
}
```

- Selector is `p.section-deck` not `.section-deck` — specificity fix for conflict with
  `.problem-text p` rule
- `font-weight: 700 !important` required — broad `p {}` rule overrides without it

---

## AI Search Visibility (2026)

Content that performs best in AI Overviews, Perplexity, and ChatGPT citations:
- Self-contained — answers the specific question completely without forward references
- Written at 8th grade level
- FAQ answers stand alone — no "as mentioned above"
- E-E-A-T signals: author credentials, specific experience claims, real location details,
  actual case types and outcomes where permitted

---

## Deprecated Schema Types (January 2026)

Do not use: Practice Problem, Dataset, Sitelinks Search Box, SpecialAnnouncement, Q&A.
