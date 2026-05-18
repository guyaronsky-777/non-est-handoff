# Day 6 — Reddit Action Queue (split into Phase A + Phase B)

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18
**Status:** awaiting Chrome action — Phase A can start immediately
**Subject:** Build the reddit_action_queue table + generator. Apply the Israeli/Middle-Eastern category-gap override for Deli Santa Maria.

---

## Important: this Day 6 is split

| Phase | What | When |
|---|---|---|
| **Phase A** | Build the infrastructure (table + n8n nodes + override rule). Test with placeholder data. | **Now — no Reddit credentials needed.** |
| **Phase B** | Re-run with real Reddit auth once Yaron provides credentials. Generate the real first Action Queue. | After Yaron sends Reddit credentials (still pending). |

Doing Phase A now while Yaron is creating the Reddit app in parallel maximises momentum.

---

## Phase A — Build the infrastructure (do this now)

### Step A1 — Create the `reddit_action_queue` table

In Supabase SQL Editor, run:

```sql
CREATE TABLE IF NOT EXISTS reddit_action_queue (
  id BIGSERIAL PRIMARY KEY,
  restaurant_id BIGINT NOT NULL REFERENCES restaurants(id) ON DELETE CASCADE,
  generated_at TIMESTAMPTZ DEFAULT NOW(),
  week_of DATE NOT NULL,
  priority INTEGER NOT NULL CHECK (priority IN (1,2,3)),
  action_type TEXT NOT NULL,
  target_signal TEXT,                  -- which Reddit signal this action addresses
  thread_url TEXT,                     -- optional: a specific thread to engage with
  subreddit TEXT,                      -- optional: which subreddit
  suggested_action TEXT NOT NULL,      -- the actual recommendation text
  rationale TEXT,                      -- why this action
  is_gap_priority BOOLEAN DEFAULT false, -- TRUE if this action came from the strategic-gap override
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending','assigned','in_progress','done','skipped')),
  assigned_to TEXT,                    -- e.g. "NON EST Manager"
  notes TEXT
);

CREATE INDEX IF NOT EXISTS idx_action_queue_restaurant_week
  ON reddit_action_queue (restaurant_id, week_of DESC);

CREATE INDEX IF NOT EXISTS idx_action_queue_status
  ON reddit_action_queue (status, week_of DESC);

COMMENT ON TABLE reddit_action_queue IS
  'Weekly per-restaurant action queue derived from Reddit Authority sub-scores.
   Each row is one concrete action the NON EST Manager should execute or assign.';
COMMENT ON COLUMN reddit_action_queue.is_gap_priority IS
  'TRUE when this action overrides the lowest-signal heuristic because the
   restaurant has a strategic category gap (e.g. Deli Santa Maria + Israeli/Middle-Eastern category).';
```

Verify:
```sql
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'reddit_action_queue'
ORDER BY ordinal_position;
```
You should see 13 columns.

### Step A2 — Build the Action Queue generator in n8n

Add a new sub-workflow `Reddit Action Queue Generator` (or extend the existing Reddit Authority Scorer with a final node). Logic:

```js
// Inputs available at this point (already produced by Reddit Authority Scorer):
//   restaurant     — full row from restaurants (including cuisine_keywords)
//   redditSignals  — the row just written to reddit_signals with all 7 sub-scores
//   weekOf         — current week start date

const result = [];

// === GAP-PRIORITY OVERRIDE ===
// Check if this restaurant has a strategic category gap.
// Rule: if cuisine_keywords.cuisine_labels contains terms with zero local-area
// competition (we proxy this by checking if Hebrew chars are present, meaning
// the restaurant has a Hebrew-tagged cuisine identity → Israeli/Middle-Eastern profile).
const cuisineLabels = (restaurant.cuisine_keywords?.cuisine_labels || []).join(' ');
const hasGapPriority = /[֐-׿]/.test(cuisineLabels) ||
                       cuisineLabels.toLowerCase().includes('israeli') ||
                       cuisineLabels.toLowerCase().includes('middle east');

if (hasGapPriority) {
  // Override: lead with the 3 gap-capture actions from the spec.
  result.push({
    priority: 1,
    action_type: 'category_capture',
    target_signal: 'strategic_gap',
    subreddit: 'r/mallorca',
    suggested_action: "Find the most recent thread in r/mallorca, r/spain, or r/europe_travel that mentions hummus, shakshuka, falafel, tahini, or 'Israeli food'. Ensure Deli Santa Maria is mentioned by an authentic community voice. Do NOT post from the owner's account.",
    rationale: "Palma currently owns the entire Israeli/Middle-Eastern category on Mallorca. Deli Santa Maria has zero inland competition for this category. This is the asymmetric win.",
    is_gap_priority: true
  });
  result.push({
    priority: 2,
    action_type: 'community_establishment',
    target_signal: 'subreddit_authority',
    subreddit: 'r/israel + r/jewish',
    suggested_action: "Identify 1 Mallorca/Spain travel thread in r/israel or r/jewish. Authentically join with the framing 'inland Mallorca Israeli deli — date syrup, tahini, real shakshuka'.",
    rationale: "Hebrew-language and Israeli-community travel discussions about Mallorca are uncontested by competitors.",
    is_gap_priority: true
  });
  result.push({
    priority: 3,
    action_type: 'flag_to_manager',
    target_signal: 'out_of_reddit_scope',
    subreddit: null,
    suggested_action: "Flag to the NON EST Manager: Hebrew-language Israeli travel forums (Tapuz, Walla Travel) — out of Reddit scope but high-value community outreach for the gap-capture strategy.",
    rationale: "Reddit scoring doesn't capture non-Reddit channels but they matter for the strategic gap. NON EST Manager handles this manually.",
    is_gap_priority: true
  });
} else {
  // === STANDARD HEURISTIC ===
  // No gap override — use the "lowest-scoring signal" heuristic.
  // Pick the lowest-scoring of the 7 signals and generate 3 actions to fix it.

  const signals = {
    volume:                 redditSignals.score_volume,
    freshness:              redditSignals.score_freshness,
    decision_threads:       redditSignals.score_decision_threads,
    subreddit_authority:    redditSignals.score_subreddit_authority,
    recommendation_density: redditSignals.score_recommendation_density,
    author_credibility:     redditSignals.score_author_credibility,
    competitor_benchmark:   redditSignals.score_competitor_benchmark
  };

  const lowestSignal = Object.entries(signals).sort((a,b) => a[1] - b[1])[0][0];

  // Action templates per signal (3 actions each, simplified)
  const TEMPLATES = {
    volume: [
      "Find 3 active threads in r/mallorca or r/spain discussing food. Identify 1 to join authentically with a relevant comment that surfaces the restaurant.",
      "Reach out to 2 local food micro-influencers who post Mallorca content. Invite to a complimentary visit. No quid-pro-quo — just access.",
      "Encourage 2 happy regular customers to share their experience on r/mallorca or r/spain when relevant threads come up."
    ],
    freshness: [
      "Find 1 thread in the last 30 days where engagement is appropriate. Authentic comment, not promotional.",
      "Post a 'what's new at the restaurant this month' update to a relevant subreddit if community norms allow.",
      "Get 1 customer to post a recent visit to a 'Mallorca recommendations this month' style thread."
    ],
    decision_threads: [
      `Identify 1 "best [X] in Mallorca" decision-stage thread to (ethically) appear in.`,
      "Search r/spaintravel for upcoming-trip threads to inland Mallorca; surface in 1 thread.",
      "Watch r/digitalnomad for 'where in Mallorca' questions; respond when relevant."
    ],
    subreddit_authority: [
      "Establish presence in 1 high-tier sub the restaurant doesn't currently appear in.",
      "Build relationship with mod / regulars of 1 niche sub for the cuisine category.",
      "Cross-reference where competitors are mentioned that the restaurant isn't, and target that sub."
    ],
    recommendation_density: [
      "Ask 2 specific regulars (post-visit) to share an authentic explicit recommendation on Reddit when context arises.",
      "Get 1 cyclist customer to mention the restaurant in r/cycling post-ride threads.",
      "Encourage user-generated content with explicit positive recommendation language."
    ],
    author_credibility: [
      "Engage with established community members (high karma, long history). Don't astroturf.",
      "Build relationships with established food/travel reviewers active on Reddit.",
      "Comment authentically from a single consistent account over time, not throwaway accounts."
    ],
    competitor_benchmark: [
      "Find 1 thread where a competitor is mentioned but this restaurant isn't. Authentic comment surfacing it.",
      "Identify which competitor leads in this segment; map their Reddit pattern; emulate (not copy).",
      "Build 1 distinctive narrative for this restaurant that the competitor doesn't have."
    ]
  };

  const actions = TEMPLATES[lowestSignal] || TEMPLATES.volume;
  actions.forEach((text, i) => {
    result.push({
      priority: i + 1,
      action_type: lowestSignal,
      target_signal: lowestSignal,
      subreddit: null,
      suggested_action: text,
      rationale: `Lowest-scoring signal is ${lowestSignal} (${signals[lowestSignal]}/100). These 3 actions target that signal.`,
      is_gap_priority: false
    });
  });
}

// === INSERT INTO TABLE ===
for (const item of result) {
  // Supabase INSERT — use the n8n Supabase node:
  await supabaseInsert('reddit_action_queue', {
    restaurant_id: restaurant.id,
    week_of: weekOf,
    priority: item.priority,
    action_type: item.action_type,
    target_signal: item.target_signal,
    subreddit: item.subreddit,
    suggested_action: item.suggested_action,
    rationale: item.rationale,
    is_gap_priority: item.is_gap_priority
  });
}
```

Add this as the **final Code node** of the Reddit Authority Scorer workflow, just after the score-write step.

### Step A3 — Run it with the existing placeholder data

Manually trigger the Reddit Authority Scorer workflow for Deli Santa Maria.

**Expected output:** 3 rows in `reddit_action_queue` for restaurant_id=1, week_of=current Monday, all 3 with `is_gap_priority = true`, all 3 from the Israeli/Middle-Eastern override (Phase A correctness check — should NOT use the lowest-signal heuristic for Deli Santa Maria).

Verify:
```sql
SELECT priority, action_type, is_gap_priority, suggested_action
FROM reddit_action_queue
WHERE restaurant_id = 1
AND week_of = date_trunc('week', CURRENT_DATE)::date
ORDER BY priority;
```

You should see exactly 3 rows, priorities 1/2/3, all `is_gap_priority = true`, all 3 about Israeli/Middle-Eastern category capture.

### Step A4 — Report back

Once Phase A is verified, write a report file at:
`from-chrome/2026-05-18-2-day6-phaseA-complete.md`

(Or, given the github.com block, paste the report body to Yaron and he'll relay.)

Content of the report:
- ✅ Table created
- ✅ Generator built
- ✅ Override logic verified — 3 rows generated, all gap_priority
- Paste the 3 `suggested_action` strings verbatim so Yaron can confirm the strategic capture text reads well
- Flag any concerns

---

## Phase B — Re-run with real Reddit auth (do this AFTER Yaron sends Reddit credentials)

Trigger: Yaron will post a new file at `from-cowork/2026-05-XX-N-reddit-credentials-ready.md` (or directly in this thread via the relay) with the n8n credential name to use.

### Step B1
Configure the n8n credential `Reddit OAuth (NON EST)` with the real `client_id` and `client_secret`. Replace the placeholder auth in the Reddit Authority Scorer workflow.

### Step B2
Re-trigger the Reddit Authority Scorer for Deli Santa Maria.

**Expected change:**
- `reddit_authority` moves from placeholder (8) to a real value, likely in the 10–30 range for Deli Santa Maria.
- `reddit_signals` row gets all 7 sub-scores populated with real data.
- `reddit_action_queue` automatically regenerates 3 fresh rows — still all gap_priority for Deli Santa Maria, but with concrete `thread_url` and `subreddit` values (not nulls) because the scorer now has real Reddit threads to point to.

### Step B3
Re-run the verification SQL from Step A3. Confirm the action queue now has real thread URLs.

### Step B4
Write a report at `from-chrome/2026-05-XX-N-day6-phaseB-complete.md` with:
- Real reddit_authority score (replaces the 8 placeholder)
- All 7 sub-score values
- The 3 action queue items with real thread URLs and subreddits
- Recommendation on whether to proceed to Sunday review or do additional iteration

---

## Why this split matters

Phase A is the **infrastructure**. It works without credentials and validates that the override rule fires correctly for Deli Santa Maria (this is the single most important behavioural check — does NON EST actually prioritize the Israeli food gap as designed?).

Phase B is the **real data**. Once credentials arrive, swapping placeholder for real auth is a 10-minute job, not a rebuild.

---

## Open question for Yaron (please relay to him from this file)

**Should NON EST ever execute the Action Queue actions automatically, or are they always Manager-reviewed first?**

My strong recommendation: **always Manager-reviewed.** Reddit's spam detection penalizes automated activity, and AI engines down-rank inauthentic accounts. The Action Queue is intelligence, never automation. But it's a product decision Yaron should make explicitly.

If the answer is "Manager-reviewed always", we add a `requires_review` column defaulting to true. We won't build this in W20, but flag the decision for W21.

---

## Acknowledge receipt

When you've read this and started Phase A, write a one-line ack at `from-chrome/2026-05-18-2-day6-ack.md` (or via Yaron):
`Day 6 instructions received. Starting Phase A — table creation.`
