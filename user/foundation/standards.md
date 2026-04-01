# Foundation — Universal Standards
## File Structure, Config, Meta, PHP Rules, Deploy

---

## File Structure

Every Foundation site ships with exactly this structure:

```
[site-root]/
  config.php
  index.php
  404.php
  sitemap.xml
  robots.txt
  .htaccess
  includes/
    header.php
    footer.php
  assets/
    css/
      styles.css
    [owner-photo].jpg
    favicon.svg
```

---

## config.php — Required Variables

```php
<?php
$site_name        = \'\';
$site_url         = \'https://domain.com\';
$owner_name       = \'\';
$owner_title      = \'\';
$phone            = \'\';
$phone_display    = \'\';
$email            = \'\';
$address_street   = \'\';
$address_city     = \'\';
$address_state    = \'\';
$address_zip      = \'\';
$address_full     = \'\';
$geo_lat          = \'\';
$geo_lng          = \'\';
$license_number   = \'\';
$years_experience = \'\';
$primary_color    = \'\';
$secondary_color  = \'\';
$accent_color     = \'\';
$font_heading     = \'\';
$font_body        = \'\';
$ga4_id           = \'\';
$gads_id          = \'\';
```

**Rule:** All contact info and brand names in variables — never hardcoded inline. Run config
alignment check before touching any page: confirm `@id` on primary schema, PostalAddress
block present, all contact info from variables not hardcoded.

---

## PHP Include Paths

- Root files: `include \'includes/header.php\';`
- Subfolder files (e.g., /locations/, /legal-marketing/): `include \'../includes/header.php\';`

**basePath rule — always set with fallback:**
```php
// Root page
$basePath = \'\';

// Subfolder page
$basePath = \'../\';

// Home link fallback
$site_url = $site_url ?: \'/\';
```

---

## PHP Quality Rules

Apply on every session, every file touched:

1. **`php -l` on every changed file.** Halt on any failure. Never deploy with a parse error.
2. **Cache flush:** `touch .htaccess` after all PHP/content changes (SiteGround dynamic cache).
3. **Apostrophes in single-quoted strings must be escaped:** Use `\'` — straight or curly
   apostrophes inside single-quoted PHP strings cause parse errors. This applies to
   `$page_title`, `$page_description`, schema strings, and all variable assignments.
4. **`$schemaMarkup` must be a raw JSON string, NOT a PHP array.**
5. **No PHP closing `?>` at end of file.** Trailing whitespace causes header already sent errors.
6. **Meta tags, canonical, and JSON-LD schema blocks belong in `<head>` — never in footer includes or below main content.**

---

## Known PHP Bugs — Check Every Session

### `$current_page = \'porch\'` Bug
Pages in content hubs were sometimes built with `$current_page = \'porch\'` instead of
`\'services\'`. Breaks nav highlighting. Check every Foundation page you touch.

Confirmed fixed on 8th Street (March 30, 2026):
- `search-engine-optimization-criminal-defense-attorneys.php`
- `criminal-lawyer-advertising.php`
- `criminal-defense-lawyer-marketing-agency.php`

Others in /legal-marketing/ may still have this bug — audit when touching them.

### Smart Apostrophes Inside PHP Strings
Word processors insert curly apostrophes (`’`). These break PHP parsing. Always use
straight `\'`. `php -l` catches this immediately.

### Data Bleed in Template Loops
If a page array or data file is shared across multiple pages, verify no meta_title or
meta_description is duplicating. Each page must have unique meta.

---

## Meta Standards

**Title format:** `Primary Keyword | Brand Name`
- Max 65 chars. Measure — never eyeball.
- Full brand name unless title would exceed limit, then shorten
- Never use pipes more than once
- Set `$page_title` before `include \'includes/header.php\';`

**Description:**
- Target 150–160 chars. Hard max 165.
- Set `$page_description` before `include \'includes/header.php\';`
- Must be unique per page — no fallback filler
- No em dashes — encoding issues across scripts. Use hyphen or reword.

**Verification scripts** must assert title and description lengths as independent values.
Identical numbers for every page = broken regex.

---

## Sitemap Standards

```xml
<url>
  <loc>https://[domain]/[slug]</loc>
  <lastmod>[YYYY-MM-DD]</lastmod>
  <priority>[0.9 or 0.8]</priority>
</url>
```

Priority guide:
- Homepage: 1.0
- Main hub pages: 0.9
- Primary query pages: 0.9
- Secondary/supporting pages: 0.8
- Legal/utility pages: 0.5 or omit

Add new entries after the last existing entry in the same section.

---

## .htaccess Redirect Patterns

```apache
# Page moves/renames
Redirect 301 /old-page /new-page
Redirect 301 /old-page.php /new-page

# Remove .php extension
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}.php -f
RewriteRule ^(.*)$ $1.php [L]
```

**Active redirects on 8th Street (reference):**
- `/results` → `/about`
- `/results.php` → `/about`
- `/contact` → `/`
- `/contact.php` → `/`
- `/services` → `/foundation`

---

## Deploy Process (SiteGround)

Remote files edited via SCP + Python scripts:
1. Write Python script locally with the Write tool
2. `scp -i ~/.ssh/id_ed25519 -P 18765 script.py u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com:/tmp/script.py`
3. `ssh -i ~/.ssh/id_ed25519 -p 18765 u3063-yznvsscqgtmo@ssh.8thstreetsolutions.com "python3 /tmp/script.py"`

Rules:
- Never bash heredocs for Python scripts — PHP `$` signs and apostrophes break them
- Use `str.replace(old, new, 1)` for targeted edits
- Always `encoding=\'utf-8\'`
- Use `\u2014` for em dash (not `\xe2\x80\x94`)

**Windows deployment:** PHP is not in the Windows PATH. Never attempt `php -l` locally on Windows. All file deployments use the base64 encode-and-decode pattern: encode locally with python3, decode on server into /tmp/, then move to final destination. Python heredocs and /dev/stdin do not work reliably on SiteGround SSH. /tmp/ file approach only. `php -l` validation always runs on the server via SSH, never locally on Windows.

**No $() command substitution in bash.** Claude Code safety filter flags it as injection risk and stops execution. Use `xargs` instead: `find . -name "*.php" | xargs -I{} php -l {}` — For loops: write to a script file first, SCP to server, then execute.

**No nested quotes in HTML attributes.** Assign to a variable first, then echo. Example: `<?php $meta_desc = htmlspecialchars($page_description ?? 'Default'); ?>` then `<meta name="description" content="<?php echo $meta_desc; ?>">` — Applies to all meta tags, canonical, OG tags, title, and any `href` or `src` that outputs a PHP variable.

---

## Current Standards (April 2026)

**Schema:** Entity markup with `sameAs` and `knowsAbout` is highest-leverage. FAQPage on
every page with FAQ content. BreadcrumbList on every interior page. JSON-LD only.

**AI search visibility:** Self-contained content at 8th grade level. FAQ answers must
stand alone.

**Content depth over breadth:** One topic thoroughly covered outperforms ten superficially.

**E-E-A-T:** Author credentials, specific experience claims, real location details, actual
case types/outcomes (where permitted).

**p.section-deck:** Every H2 gets one. Selector is `p.section-deck`. `!important` on
font-weight.

**PHP quality gate:** `php -l` every file. `touch .htaccess` after every deploy.

**Deprecated (January 2026):** Practice Problem, Dataset, Sitelinks Search Box,
SpecialAnnouncement, Q&A schema types.

**BreadcrumbList insertion** — three patterns exist. Identify the pattern before inserting:

**Pattern 1 — @graph pages (most common):** BreadcrumbList is a new node inside `@graph`. The closing structure is two lines — first bracket closes `@graph`, second closes `json_encode` array. Insert BEFORE the `@graph` close. Losing the `@graph` bracket puts BreadcrumbList outside `@graph` — PHP parses but schema is invalid.

**Pattern 2 — Flat single block (no @graph):** Append as a separate concatenated block. BreadcrumbList needs its own `@context` since it is a standalone block. Use: `$schema_json .= "\n" . $breadcrumb_json;`

**Pattern 3 — Dual flat (two blocks concatenated):** Add as a third block in the same concatenation line. Same `@context` requirement as Pattern 2.

After the first session on any site, record which pattern each page uses in the site's `context.md` under a **Schema Patterns** section — eliminates the grep discovery pass on every future session.
