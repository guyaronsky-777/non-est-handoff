# Day 7 — Sunday Review (24 May 2026) — Template

**From:** Claude in Cowork
**To:** Claude in Chrome + Yaron
**Date:** to be completed Sunday 24 May 2026
**Status:** template — Chrome fills in numbers + observations, Yaron + Cowork decide Week 21
**Subject:** Week 20 closeout + Week 21 decisions

---

## How to use this template

Sunday morning, Chrome assistant fills in **Section 1 (numbers)** and **Section 2 (observations)** by querying Supabase. Yaron and Cowork then go through **Section 3 (decisions)** together in a 60-min call. Final output: this file becomes `REVIEW-week20-summary.md` (committed to the bridge AND saved to `Non Est/build/2026-W20-scoring-upgrade/`).

---

## Section 1 — Numbers (Chrome fills in)

### Definition-of-Done checks (the 5 acceptance criteria)
- [ ] Supabase has 2 new columns, 2 new detail tables, 1 new view, 1 weights config table — `___ ✅ / ❌`
- [ ] n8n has working Reddit Authority Scorer + Agentic A/C scorer + Action Queue generator — `___ ✅ / ❌`
- [ ] Deli Santa Maria has ≥1 full 12-category audit row, all sub-scores populated, `scoring_version='v2.0-12cat'` — `___ ✅ / ❌`
- [ ] Retool displays 12 categories + Reddit breakdown + Agentic breakdown + Action Queue — `___ ✅ / ❌`
- [ ] Yaron + Claude completed Day-7 review (this document) — `___ ✅ / ❌`

### Deli Santa Maria final scorecard (latest audit, v2.0-12cat)

| Category | Score (0–100) | Notes |
|---|---|---|
| overall_score_v2 | _____ | |
| google_dominance | _____ | |
| website_seo | _____ | |
| directory_coverage | _____ | |
| social_presence | _____ | |
| review_engine | _____ | |
| booking_ordering | _____ | |
| visual_assets | _____ | |
| geo_ai_readiness | _____ | |
| paid_visibility | _____ | |
| content_authority | _____ | |
| **reddit_authority** | _____ | placeholder=8 OR real=___ |
| **agentic_readiness** | _____ | |

### Reddit Authority sub-scores

| Signal | Score | What it means |
|---|---|---|
| Volume (12mo mentions) | _____ | |
| Freshness (last 90d) | _____ | |
| Decision-stage threads | _____ | |
| Subreddit authority | _____ | |
| Recommendation density | _____ | |
| Author credibility | _____ | |
| Competitor benchmark | _____ | |

### Agentic Readiness components

| Component | Status | Score |
|---|---|---|
| Menu format | _____ (html_with_prices / pdf / image / ...) | _____ /100 |
| Schema completeness | ✅/❌ Restaurant, ✅/❌ FAQ, ✅/❌ Menu, ✅/❌ Reservation, ✅/❌ Offer, ✅/❌ AggregateRating | _____ /100 |

### Reddit Action Queue — the 3 generated actions

1. **Priority 1:** _________________ (paste suggested_action verbatim) — `is_gap_priority: true/false`
2. **Priority 2:** _________________
3. **Priority 3:** _________________

---

## Section 2 — Observations (Chrome fills in)

### What worked well in Week 20
- (Chrome's observations)

### What was slower than planned
- (Chrome's observations)

### Surprises (good and bad)
- (Chrome's observations)

### Risk register update
- Reddit credentials still pending (status: blocked Sun 18 May, retry tomorrow)
- GitHub token regen not actually completed (low priority — old token still works, repo is public)
- Day 6 Phase B (real Reddit data) still pending

---

## Section 3 — Decisions (Yaron + Cowork)

### Decision 1 — Week 21 priority order

Pick the top 2–3 for next week. (Discussion guide below for context.)

- [ ] **Agentic Component B** (bookability — Reservation schema + TheFork/OpenTable detection). Tightens the agentic moat, ~5 days.
- [ ] **Agentic Component D — scoping** (real-agent simulation — needs OpenAI/Anthropic/Google/Perplexity API spend). Decide budget envelope first.
- [ ] **Run new audit on Ca Na Toneta** (id=2). One day. Validates the system works for a 2nd restaurant before we commit to clients.
- [ ] **Start the Avatar Engine** (begin populating the 7-layer Identity/Operational/GEO/Media/Reputation/Technical/Distribution truth object for Deli Santa Maria). Big effort, real product begins here.
- [ ] **Start the sales funnel** (free-audit landing page → demo → first paying client). Pushes the moat work to the back-burner but starts learning from real prospects.
- [ ] **Drift detection workflow** (table exists; build the n8n daily job that detects changes in Deli Santa Maria's online presence and writes to drift_alerts).
- [ ] **Per-segment Reddit scoring** (already designed in spec Addendum A1; full implementation moves from Day-6 partial to fully integrated).

### Decision 2 — Reddit/Agentic credentials & data sources

- If Reddit API still won't work tomorrow, switch to Plan B: ___ (Reddit's new developer portal at developers.reddit.com / a paid Reddit data provider e.g. ScrapingBee, ~$30–100/mo / skip Reddit data entirely and use search-engine proxies)
- Decide budget for Day-6 Phase B + Agentic Component D: $ ___ /month

### Decision 3 — Should action queue actions ever be automated?

Cowork's strong recommendation: **always Manager-reviewed, never automated.**
Reddit spam detection penalizes automation; AI engines down-rank inauthentic accounts.

Yaron decides: `[ ] always manager-reviewed (recommended)` / `[ ] some auto-execute (specify which) ___`

### Decision 4 — When to onboard a 2nd restaurant

Options:
- [ ] After Avatar Engine v1 is built for Deli Santa Maria (proves the model on a single client first)
- [ ] Immediately — pick a paying client, even if Avatar Engine isn't there yet, deliver basic audit + recommendations
- [ ] After we have the free-audit landing page (let prospects come to us)

### Decision 5 — Sunday review cadence going forward

- [ ] Weekly Sunday reviews (every week, same template)
- [ ] Only at milestone moments
- [ ] Different cadence: ___

---

## Section 4 — Next-week plan (auto-generated from Section 3)

Once decisions in Section 3 are made, Cowork writes:
`Non Est/build/2026-W21-[chosen-focus]/00-overview.md`

Containing:
- Definition of done for Week 21
- 7-day plan
- Risk register
- Assistant handoff (updated handoff doc for Chrome)

---

## Section 5 — Quick wins to take into Week 21

(Things we noticed in Week 20 that aren't worth a full week but should be done somewhere)

- (Yaron + Chrome add bullets here)

---

*Template prepared 18 May 2026 by Claude in Cowork. To be completed Sunday 24 May 2026.*
