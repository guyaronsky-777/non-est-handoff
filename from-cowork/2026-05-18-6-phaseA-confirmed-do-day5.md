# Phase A confirmed by Cowork — proceed to Day 5 (Retool dashboard)

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18
**Status:** Phase A ✅ verified. Day 5 is your next active task.
**Subject:** Lock decisions + your next active task

---

## Phase A confirmed — congratulations on the bonus finding

Your Phase A report verifies that the engine handles **both** the override path AND the standard heuristic path correctly:

| Restaurant | Triggered rule | Output |
|---|---|---|
| Deli Santa Maria | Gap override (Hebrew/Israeli labels detected) | 3 rows × `is_gap_priority=true` × Israeli capture actions |
| Ca Na Toneta | Standard heuristic (lowest-signal = volume) | 3 rows × `is_gap_priority=false` × volume-targeting actions |

Both paths firing correctly is the proof-of-concept that Week 20 was structured to achieve. ✅

## Decision locked: Action Queue is ALWAYS Manager-reviewed

Yaron + Cowork have agreed. The behavior change for you this week is **none** — keep building as-is. The change applies in Week 21:

- Add `requires_review BOOLEAN DEFAULT true` column to `reddit_action_queue` (W21 SQL migration)
- UI treatment in Retool: when displaying queue rows, the "Mark done" button stays — but no automation hooks. Ever.

Add this to your Week 21 todo. Do not change anything in `reddit_action_queue` this week.

---

## Your next active task: Day 5 — Retool dashboard

Full instructions are already in the bridge at:
**`https://raw.githubusercontent.com/guyaronsky-777/non-est-handoff/main/from-cowork/2026-05-18-4-day5-retool-dashboard.md`**

Read it end-to-end before starting. Brief summary so you have context here too:

### What Day 5 builds

A Retool page for selected restaurant showing:
1. **Overall Score** tile bound to `audit_scores_v2.overall_score_v2` (not the old `overall_score`)
2. **12 category tiles** (the original 10 + new Reddit Authority + Agentic Readiness)
3. **Reddit Authority — Breakdown panel** showing the 7 sub-scores from `reddit_signals` as progress bars
4. **Agentic Readiness — Breakdown panel** showing menu format + 7 schema checkboxes from `agentic_signals`
5. **Reddit Action Queue panel** showing the 3 generated actions with "Mark done" buttons

### Why this matters

Yaron can't make decisions on Sunday's review (Day 7) about numbers he can't see. The Retool dashboard is what turns the data we've built into something he can look at and act on.

### Time estimate

3–4 hours. Most is layout fiddling; the queries are simple.

### Special notes

- Match existing visual style — same fonts, same colors, same tile size
- Flag the Reddit Authority score visually somehow (small "placeholder" badge?) so Yaron knows it's the 8 placeholder until Phase B runs with real Reddit auth
- The Action Queue panel should highlight `is_gap_priority=true` rows distinctly (a "⭐ Strategic Gap" chip in a different color)

### Report back when done

Take a screenshot of the dashboard with Deli Santa Maria selected. Paste the description (or the screenshot, if Yaron can relay it) so Yaron + Cowork can review on Sunday.

---

## What Day 5 is NOT

- ❌ Not "polished for client-facing demos" — that's Week 22+
- ❌ Not all restaurants — Deli Santa Maria + Ca Na Toneta only
- ❌ Not the Avatar Engine — separate work, Week 21+
- ❌ Not connected to any external system — internal-facing only

---

## What's still pending (don't touch this week)

- **Day 6 Phase B** — blocked on Reddit credentials. Yaron retrying tomorrow. The current `reddit_authority = 8` is a placeholder; when Phase B fires, it becomes a real value (likely 10–30 for Deli Santa Maria).
- **Drift detection workflow** — Week 21.
- **Sales funnel / free-audit landing page** — separate decision later.
- **Avatar Engine** — biggest unbuilt thing in the vision. Week 21+ if Yaron prioritizes it.

---

## Communication

- End-of-day report at 18:00 CET via WhatsApp, same template as before.
- Next major handoff via the bridge: Reddit credentials (whenever Yaron sorts the Reddit app), then Phase B instructions, then Sunday review prep.
- If you hit a blocker mid-Day-5, write to `from-chrome/` (or relay via Yaron) — don't sit idle.
