# Deli Santa Maria — Wix Quick Fixes (DUAL POSITIONING — supersedes prior version)

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18 (revised after Yaron locked dual positioning decision)
**Status:** awaiting Chrome execution AFTER Day 5 Retool dashboard finishes
**Subject:** Apply 4 surgical edits to https://www.deli-caffe.com using the DUAL POSITIONING approach.

---

## ⚠️ This file has been revised — read the strategy first

**Previous draft assumed "Israeli-first everywhere." That's been changed.**

The locked positioning is **dual, context-layered:**
- Schema captures both **Middle Eastern + Israeli** (so AI engines surface the restaurant for both query pools)
- Public-facing English/Spanish/German copy leads with **"Middle Eastern & Israeli"** (lower geopolitical friction in current 2026 climate)
- Hebrew touchpoints lead with **"Israeli"** (authentic, captures Hebrew-language demand)

Canonical sentence: **"A Middle Eastern & Israeli kitchen — the way it's eaten at home. Real food, real hands, daily."**

---

## Edit #1 — Page title tag

**Where:** Wix Editor → home page → SEO Basics → Page Title field
**Current:** `Deli Santa Maria | Mediterranean kitchen | Mallorca`
**Change to:** `Deli Santa Maria | Middle Eastern & Mediterranean Kitchen | Mallorca`

Notes:
- Leads with "Middle Eastern" (broader appeal, lower friction).
- "Israeli" is NOT in the title — it's deeper in schema + alternateName. Title is the most visible public surface.
- "Mediterranean" stays as the secondary anchor to retain existing SEO equity.

## Edit #2 — Page meta description

**Where:** Wix Editor → home page → SEO Basics → Meta Description field (limit ~155 chars)
**Current:** `Mediterranean restaurant, Takeaway & catering in Santa María del Camí, Mallorca. Local specialities, vegan-friendly brunch, dog-friendly terrace. Mon–Fri 9–16, Sun 9–15.`
**Change to (153 chars):** `Inland Mallorca's Middle Eastern & Israeli kitchen — shakshuka, hummus, tahini. Real daily cooking, not a hotel concept. Vegan-friendly. Mon–Fri 9–16.`

Notes:
- "Middle Eastern" leads, "Israeli" present.
- Names signature dishes (shakshuka, hummus, tahini) — direct entity terms for AI matching.
- "Real daily cooking, not a hotel concept" is the differentiation against NENI Mallorca.

## Edit #3 — Schema `servesCuisine` and related fields (CRITICAL — biggest impact)

**Where:** Wix's schema is at the bottom of the home page source as a JSON-LD `<script>`. Manage via Wix Editor → page SEO → Advanced → Add structured data, OR via Settings → Custom Code (if needed).

**Current schema:**
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Deli Santa Maria",
  "url": "https://www.deli-caffe.com",
  "telephone": "+34628008066",
  "priceRange": "€€",
  "servesCuisine": ["Mediterranean","Vegetarian","Brunch","Breakfast"],
  "address": {...},
  "geo": {...},
  "openingHours": ["Mo-Fr 09:00-16:00","Su 09:00-15:00"],
  "aggregateRating": {"@type":"AggregateRating","ratingValue":"4.8","reviewCount":"400"}
}
```

**Change to:**
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Deli Santa Maria",
  "alternateName": ["דלי סנטה מריה","Deli SM","Santa Maria Deli"],
  "description": "Inland Mallorca's Middle Eastern & Israeli kitchen — real shakshuka, hummus from scratch, real tahini, signature date-syrup pastries. Vegan-friendly daily cooking. The way it's eaten at home.",
  "url": "https://www.deli-caffe.com",
  "telephone": "+34628008066",
  "priceRange": "€€",
  "servesCuisine": ["Middle Eastern","Israeli","Mediterranean","Vegetarian","Brunch","Breakfast"],
  "keywords": "Middle Eastern food Mallorca, Israeli food Mallorca, shakshuka, hummus, tahini, inland Mallorca, brunch, deli, vegan-friendly, dog-friendly, authentic kitchen",
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

**Three things changed from current:**
1. `servesCuisine` array now starts with **"Middle Eastern"** then **"Israeli"** then "Mediterranean" — captures both query pools, leads with the broader (less politically-charged) frame.
2. `alternateName` includes the Hebrew name `דלי סנטה מריה` for Hebrew-language search demand.
3. `description` and `keywords` mention both Middle Eastern + Israeli, name signature dishes, and reference "inland Mallorca" + "authentic kitchen" for the NENI-rival positioning.

## Edit #4 — Home page body paragraph

**Where:** Wix Editor → home page text section (the section currently saying "A Fresh Mediterranean Kitchen in Santa Maria del Camí")

**Current:**
> *"At Deli Santa Maria we are a Mediterranean restaurant serving vibrant food inspired by local ingredients and Middle Eastern flavors — fresh, simple, and full of life."*

**Change to:**
> *"At Deli Santa Maria we are inland Mallorca's Middle Eastern & Israeli kitchen — the way it's eaten at home. Shakshuka cooked daily, hummus from scratch, real tahini, signature date-syrup pastries. Real food, real hands, daily."*

Notes:
- "Middle Eastern & Israeli" leads (dual frame, public-facing).
- Names 4 signature dishes (shakshuka, hummus, tahini, date-syrup pastries).
- "the way it's eaten at home" + "Real food, real hands, daily" — the differentiation against NENI's hotel-rooftop scene.
- "inland Mallorca" — geographic positioning.

---

## Verification steps after edits

1. **Publish** the changes in Wix Editor (click Publish, not just Save).
2. **Wait 2 minutes** for Wix's CDN to propagate.
3. **Fetch the live site** in an incognito tab. View Page Source. Search (Ctrl+F) for:
   - `Middle Eastern` — should appear in title, meta description, and the JSON-LD `servesCuisine` array (first item)
   - `Israeli` — should appear in description (schema), keywords (schema), alternateName Hebrew, and `servesCuisine` array (second item)
   - `שקשוקה` or `דלי` — Hebrew alternateName visible in schema
4. **Schema validator:** https://search.google.com/test/rich-results — paste the URL, run it. Should confirm Restaurant schema with no errors.

---

## Report back via Yaron (since github.com is blocked for you)

```
# Wix Quick Fixes — Live (Dual Positioning)

✅/❌ Edit #1 — title tag changed to "Middle Eastern & Mediterranean Kitchen"
✅/❌ Edit #2 — meta description rewritten with "Middle Eastern & Israeli"
✅/❌ Edit #3 — schema servesCuisine = [Middle Eastern, Israeli, Mediterranean, ...]; description + keywords + Hebrew alternateName all present
✅/❌ Edit #4 — body paragraph rebalanced

✅/❌ Site re-published and live
✅/❌ Schema validator passes
✅/❌ Page source confirms both "Middle Eastern" and "Israeli" present

Notes / issues: ___

Time taken: ___ minutes
```

---

## Things to NOT do in this session

- ❌ Do not change the menu page (separate work)
- ❌ Do not touch booking/contact flow
- ❌ Do not modify photos or design
- ❌ Do not add new pages
- ❌ Do not change Wix settings outside SEO Basics + page meta + schema

These 4 edits are surgical. Anything else risks breaking the live site for unrelated changes.
