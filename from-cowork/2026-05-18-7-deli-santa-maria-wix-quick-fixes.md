# Deli Santa Maria — Wix Quick Fixes (high leverage, ~30 min)

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18
**Status:** awaiting Chrome execution. Can run after Day 5 Retool work, OR Yaron may interrupt Day 5 if he wants these live tonight.
**Subject:** Apply 4 surgical edits to https://www.deli-caffe.com to fix the biggest AI misclassification finding from today's Mirror Report.

---

## Context — why this matters

Today's AI Mirror demo (file: `Non Est/AI-MIRROR-DEMO_Deli_Santa_Maria_2026-05-18.md`) found that every AI engine (Gemini, ChatGPT, Perplexity, Claude) categorizes Deli Santa Maria as a generic Mediterranean restaurant — not as the Israeli/Middle-Eastern fusion deli it actually is. The root cause: the Wix site's schema, title tag, and meta description all lead with "Mediterranean" and never mention "Israeli" or "Middle Eastern" prominently.

These 4 edits change how the site is interpreted by AI engines on the next crawl. Expected effect: within 2–3 weeks, queries like "best Israeli food Mallorca" and "shakshuka Mallorca" start surfacing Deli Santa Maria instead of only Palma competitors.

Site is hosted on Wix (Wix.com Website Builder per the page's `meta-generator` tag). Yaron is the owner — log into Wix dashboard with his session.

---

## Edit #1 — Page title tag

**Where:** Wix Editor → home page → SEO Basics (or the SEO tools panel) → Page Title field
**Current:** `Deli Santa Maria | Mediterranean kitchen | Mallorca`
**Change to:** `Deli Santa Maria | Israeli & Mediterranean Kitchen | Mallorca`

This is the single most weighted field by AI engines. Just adding "Israeli &" before "Mediterranean" repositions the entire page.

## Edit #2 — Page meta description

**Where:** Wix Editor → home page → SEO Basics → Meta Description field (limit ~155 characters)
**Current:** `Mediterranean restaurant, Takeaway & catering in Santa María del Camí, Mallorca. Local specialities, vegan-friendly brunch, dog-friendly terrace. Mon–Fri 9–16, Sun 9–15.`
**Change to (149 chars):** `Inland Mallorca's authentic Israeli & Mediterranean deli — shakshuka, hummus, tahini. Vegan-friendly brunch. Santa Maria del Camí. Mon–Fri 9–16.`

This is the second most weighted field. Leading with "authentic Israeli" + naming signature dishes (shakshuka, hummus, tahini) gives AI engines specific entity terms to match against user queries.

## Edit #3 — Schema `servesCuisine` (CRITICAL — biggest impact)

**Where:** This is JSON-LD schema embedded in the page source. In Wix, schema is typically managed via one of:
- **Wix Editor → page → SEO Basics → Advanced → "Add structured data"** (some Wix plans have this UI)
- **Wix Editor → Settings → Custom Code → "Add Custom Code"** with a `<script type="application/ld+json">` block placed in `<head>` of the home page only
- **Velo (Wix code)** if a developer has set it up
- **Wix's auto-generated Restaurant schema** if the business type is set to Restaurant in business profile

**Current schema (from page source):**
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Deli Santa Maria",
  "url": "https://www.deli-caffe.com",
  "telephone": "+34628008066",
  "priceRange": "€€",
  "servesCuisine": ["Mediterranean","Vegetarian","Brunch","Breakfast"],
  ...
}
```

**Change to:**
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Deli Santa Maria",
  "alternateName": ["דלי סנטה מריה","Deli SM","Santa Maria Deli"],
  "description": "The only authentic Israeli & Middle-Eastern deli in Mallorca's interior. Fresh shakshuka, hummus, tahini fusions, signature date-syrup pastries, vegan-friendly brunch.",
  "url": "https://www.deli-caffe.com",
  "telephone": "+34628008066",
  "priceRange": "€€",
  "servesCuisine": ["Israeli","Middle Eastern","Mediterranean","Vegetarian","Brunch","Breakfast"],
  "keywords": "Israeli food Mallorca, shakshuka, hummus, tahini, Middle Eastern, inland Mallorca, brunch, deli, vegan-friendly, dog-friendly, kosher-style",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Carrer de l'Església, 13",
    "addressLocality": "Santa Maria del Camí",
    "addressRegion": "IB",
    "postalCode": "07320",
    "addressCountry": "ES"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 39.6476469,
    "longitude": 2.7792455
  },
  "openingHours": ["Mo-Fr 09:00-16:00","Su 09:00-15:00"],
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "reviewCount": "400"
  }
}
```

**Three changes from current to new:**
1. `servesCuisine` array prepended with `"Israeli"` and `"Middle Eastern"` (before "Mediterranean")
2. New `alternateName` array including the Hebrew name (`דלי סנטה מריה`) for Hebrew-language Israeli traveler searches
3. New `description` and `keywords` fields explicitly stating the category and signature dishes

**Implementation note for Chrome:** If Wix's UI doesn't let you edit the schema directly, the fallback path is via **Settings → Custom Code → Add Custom Code**, target = `<head>`, applies to = Home page only. Paste the new full schema as `<script type="application/ld+json">...</script>`. If a Wix-auto-generated schema is still firing, set the new custom one to load AFTER the auto-generated one (Wix uses the last-loaded JSON-LD).

## Edit #4 — One sentence rebalance in home page body copy

**Where:** Wix Editor → home page text section (the section that currently says "A Fresh Mediterranean Kitchen in Santa Maria del Camí")
**Current text (first paragraph after the hero):**
> *"At Deli Santa Maria we are a Mediterranean restaurant serving vibrant food inspired by local ingredients and Middle Eastern flavors — fresh, simple, and full of life."*

**Change to:**
> *"At Deli Santa Maria we are inland Mallorca's authentic Israeli & Mediterranean deli — shakshuka cooked daily, hummus from scratch, real tahini, signature date-syrup pastries, and the warm welcome of a real Israeli kitchen."*

The change does 3 things at once:
- Promotes "Israeli" to first cuisine word (previously zero mentions on home page)
- Names 4 signature Middle-Eastern dishes
- Adds the "inland Mallorca" geo-positioning that captures the strategic gap

---

## Verification steps after edits

1. **Publish the changes** in Wix Editor (click Publish, not just Save).
2. **Wait 2 minutes** for Wix's CDN to propagate.
3. **Fetch the live site** to verify the new content is live. Open https://www.deli-caffe.com in an incognito browser tab. Right-click → View Page Source. Search (Ctrl+F) for:
   - `Israeli` — should now appear in the visible body, in the title tag, AND in the JSON-LD schema
   - `servesCuisine` — verify the array now starts with `"Israeli","Middle Eastern",...`
   - `alternateName` — verify the Hebrew name appears
4. **Test the Google rich-results / schema validator:** https://search.google.com/test/rich-results — paste the URL, run it. Should report Restaurant schema valid with the new fields.

---

## Report back

When the 4 edits are live and verified, write back to Yaron (he'll relay to Cowork):

```
# Wix Quick Fixes — Live

✅/❌ Edit #1 — title tag changed
✅/❌ Edit #2 — meta description changed
✅/❌ Edit #3 — schema servesCuisine updated to include Israeli + Middle Eastern
✅/❌ Edit #4 — body copy rebalanced (1 paragraph)
✅/❌ Site re-published and live
✅/❌ Schema validator confirms no errors

Notes / issues: ___

Time taken: ___ minutes
```

---

## Things to NOT do in this session

- ❌ Do not change the menu page (separate work)
- ❌ Do not change the booking/contact flow (separate work)
- ❌ Do not touch the photos or design (separate work)
- ❌ Do not add new pages (the "Israeli food in Mallorca" dedicated landing page is a Week 21 deliverable, not now)
- ❌ Do not modify any Wix settings unrelated to SEO Basics / page meta / schema

These 4 edits are the surgical wins. Anything else risks breaking the live site for unrelated changes.

---

## Why these 4 specifically

From today's AI Mirror Report on Deli Santa Maria, these are the 4 highest-leverage CLEAN-bucket fixes. Combined, they retrain AI engines to categorize the restaurant correctly within 2–3 weeks. Everything in the STRENGTHEN and ADD buckets is more work; these 4 are the 80/20.
