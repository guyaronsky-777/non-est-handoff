# Day 5 — Retool Dashboard (12 categories + signal breakdowns)

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18
**Status:** awaiting Chrome execution
**Subject:** Build the Retool dashboard upgrade for the v2.0-12cat scoring. Yaron needs this for Sunday's review — he can't look at numbers that aren't visible.

---

## Goal

After this work, opening https://nonest.retool.com and selecting Deli Santa Maria should show:

1. A single big **Overall Score** tile (using `audit_scores_v2.overall_score_v2`, not the old `overall_score`)
2. **12 category tiles** in a grid (not 10)
3. A **"Reddit Authority — Breakdown"** panel showing the 7 sub-scores
4. An **"Agentic Readiness — Breakdown"** panel showing menu format + schema checklist
5. A **"Reddit Action Queue — This Week"** panel showing the 3 actions

---

## Step 1 — Update the Overall Score tile

Find the existing Overall Score tile on the main restaurant page.

Change its query from `audit_scores` to `audit_scores_v2`:

```sql
SELECT overall_score_v2, scoring_version
FROM audit_scores_v2
WHERE restaurant_id = {{ selectedRestaurant.id }}
ORDER BY created_at DESC
LIMIT 1;
```

Bind the displayed value to `overall_score_v2`. Add a small footer text below the tile: `{{ query.data.scoring_version }}` (it should display `v2.0-12cat`).

## Step 2 — Add 2 new score tiles

Duplicate any existing category score tile twice. For each duplicate:

**Tile A — Reddit Authority**
- Label: "Reddit Authority"
- Query field: `reddit_authority` from `audit_scores`
- Add a small star/icon if `reddit_authority` is currently a placeholder (you can check `reddit_notes` for the word "placeholder")
- Color rules: 0–30 red, 31–60 amber, 61–100 green

**Tile B — Agentic Readiness**
- Label: "Agentic Readiness"
- Query field: `agentic_readiness` from `audit_scores`
- Color rules: 0–30 red, 31–60 amber, 61–100 green

Position them adjacent to the existing `geo_ai_readiness` tile (they're conceptually related).

## Step 3 — Add the "Reddit Authority — Breakdown" panel

New container. Query:

```sql
SELECT
  score_volume, score_freshness, score_decision_threads,
  score_subreddit_authority, score_recommendation_density,
  score_author_credibility, score_competitor_benchmark,
  mention_count_12mo, mention_count_90d,
  segment_breakdown, top_mentions_evidence
FROM reddit_signals
WHERE restaurant_id = {{ selectedRestaurant.id }}
ORDER BY measured_at DESC
LIMIT 1;
```

Render:
- 7 horizontal progress bars, one per sub-score, labeled in plain English ("How much do people mention this restaurant on Reddit?" / "Are mentions recent?" / etc.)
- Below the bars: small text "12-month mentions: X | Last 90 days: Y"
- If `segment_breakdown` is non-null, render it as a 4-row table (one per segment with the segment label and its benchmark_score)
- If `top_mentions_evidence` is non-null, render the first 3 entries as clickable links to the Reddit threads

## Step 4 — Add the "Agentic Readiness — Breakdown" panel

New container. Query:

```sql
SELECT
  menu_url, menu_format, score_menu_format,
  schema_restaurant_present, schema_localbusiness_present,
  schema_faqpage_present, schema_menu_present,
  schema_reserve_action_present, schema_offer_present,
  schema_aggregate_rating_present, score_schema_completeness
FROM agentic_signals
WHERE restaurant_id = {{ selectedRestaurant.id }}
ORDER BY measured_at DESC
LIMIT 1;
```

Render:
- One line: **"Menu format:** {{ menu_format }} (score: {{ score_menu_format }}/100)"
- If `menu_url` is non-null, render as clickable link
- A checklist of 7 schema items, each as ✅ green or ❌ red:
  - Restaurant schema
  - LocalBusiness schema (fallback)
  - FAQ schema
  - Menu schema
  - Reservation action schema
  - Offer schema
  - Aggregate rating schema
- Below the checklist: "Schema completeness: {{ score_schema_completeness }}/100"

## Step 5 — Add the "Reddit Action Queue — This Week" panel

Should only appear once the `reddit_action_queue` table is populated (Phase A of Day 6 must be complete first).

New container titled "Action Queue — This Week". Query:

```sql
SELECT priority, action_type, subreddit, suggested_action, rationale, is_gap_priority, status
FROM reddit_action_queue
WHERE restaurant_id = {{ selectedRestaurant.id }}
AND week_of = date_trunc('week', CURRENT_DATE)::date
ORDER BY priority;
```

Render as 3 cards stacked vertically:
- Each card has the **priority** number prominent on the left
- The **action_type** as a small chip/tag at top
- If `is_gap_priority = true`, add a special chip "⭐ Strategic Gap" in a distinct color
- The **suggested_action** as the main body text
- The **rationale** as smaller italic text below
- A "Mark done" button on the right — when clicked, updates the row's `status` to `'done'` via:

```sql
UPDATE reddit_action_queue
SET status = 'done', notes = {{ optional_notes }}
WHERE id = {{ this_row.id }};
```

## Step 6 — Page-level scoring version footer

At the very bottom of the page, add a small text component:
`Scoring model: v2.0-12cat | Last audit: {{ audit_scores_v2.data.created_at }}`

So future-Yaron always knows what version of the model he's looking at.

## Step 7 — Test on Deli Santa Maria

Select Deli Santa Maria from the restaurant picker. Verify:
- Overall score shows 52 (or whatever `overall_score_v2` reports for the most recent audit)
- All 12 category tiles populate
- Reddit Authority breakdown shows 8 (placeholder) — flag this visually so Yaron knows it's not real data yet
- Agentic Readiness breakdown shows menu_format + schema checklist
- Action Queue shows 3 gap-priority actions (only after Phase A of Day 6 is done)

## Step 8 — Report

Take a screenshot of the dashboard, or describe in writing what each section shows. Paste to Yaron — he'll relay to me. Specifically confirm:

- ✅/❌ All 12 tiles visible
- ✅/❌ Reddit breakdown panel visible
- ✅/❌ Agentic breakdown panel visible
- ✅/❌ Action Queue panel visible (if Phase A is done)
- ✅/❌ Old `overall_score` vs new `overall_score_v2` discrepancy is visible/explained

---

## Style notes

- Match existing visual style — don't introduce new fonts/colors
- The dashboard's first goal is **clarity for Yaron**, not impressing clients. Plain labels, big readable numbers
- We'll polish for client-facing demos later (probably Week 22+)

## Time estimate

~3–4 hours. Most of it is layout fiddling. The queries are simple.
