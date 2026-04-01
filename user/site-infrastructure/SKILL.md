---
name: site-infrastructure
description: "Diagnose, fix, and maintain the technical infrastructure layer of any Foundation site. Use when working on 404 pages, sitemap.xml, robots.txt, .htaccess, config.php, or header/footer includes — independent of any build or SEO session. Triggers on: broken redirects, missing sitemap, crawl errors, include errors, config mismatches, htaccess rules, site not loading correctly."
---

# Site Infrastructure

The technical foundation every Foundation site requires to function correctly. This skill is called independently — not only during builds or SEO sessions.

**Client-specific notes in:** `clients/[client-id]/context.md` on SiteGround.

---

## The Six Components

### 1. config.php
Single source of truth for all site variables. No contact info, URLs, or brand names hardcoded inline anywhere.

**Check:** Config alignment before touching any page — confirm all variables populated, no empty strings going live.

### 2. .htaccess
Handles redirects, HTTPS enforcement, www/non-www canonicalization, and security headers.

**Standard rules every site needs:**
- Force HTTPS
- Canonicalize www vs non-www (pick one, redirect the other)
- 301 redirects for any changed URLs
- Block access to config.php and sensitive files

### 3. 404.php
Custom 404 page. Must load without PHP errors, include header/footer, and offer navigation back to homepage and primary service pages.

**Check:** Return actual 404 status code — not 200.

### 4. sitemap.xml
One URL per page. No duplicates. Must match actual live page count.

**Check:** URL count in sitemap matches page count on server. New pages added same session they are built.

### 5. robots.txt
Allow Googlebot. Disallow /includes/, any admin paths. Point to sitemap URL.

**Standard:**
```
User-agent: *
Disallow: /includes/
Sitemap: https://[domain]/sitemap.xml
```

### 6. includes/ — header.php + footer.php
Shared across all pages. PHP errors here break every page simultaneously.

**Check:** PHP lint on both files before any edit. Confirm config.php is required at top of header.php.

---

## Diagnostic Sequence

When something is broken and the component is not obvious:
```bash
# PHP lint sweep
php -l *.php includes/*.php

# Check 404 response code
curl -o /dev/null -s -w "%{http_code}" https://[domain]/nonexistent-page

# Sitemap URL count
grep -c '<loc>' sitemap.xml

# Junk/backup files
find public_html -name "*.zip" -o -name "*.bak" -o -name "php_errorlog"
```

---

## Fit With Other Skills

- **Foundation** owns the build — this skill owns the infrastructure layer independently
- **Curb Appeal** runs Step 0 site audit — if errors found, this skill is what gets called
- **Client data** in `clients/[client-id]/context.md` — never in this skill

---

*Site Infrastructure — 8th Street Solutions*
*Methodology only. Client data in clients/[client-id]/ on SiteGround.*
*Last updated: April 1, 2026*
