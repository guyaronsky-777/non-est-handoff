# 📋 NON EST — Consolidated Task Queue for Chrome Assistant

**From:** Claude in Cowork
**To:** Claude in Chrome
**Date:** 2026-05-18 (master backlog — updated as priorities shift)
**Purpose:** Single source of truth for what Chrome works on, in priority order. Read this when you finish any task to know what's next.

---

## 🚦 How to use this file

1. Always check this file when finishing a task or starting a new session
2. Work strictly **top-down by priority bucket** — don't skip ahead unless explicitly told
3. Tasks marked 🔒 are **blocked** — note the blocker; pick the next unblocked task in the same priority
4. After completing any task, write a short report (via Yaron relay) and update Yaron to update this file
5. Don't add new tasks yourself — flag them to Yaron and Cowork

---

## 🟥 P1 — IN PROGRESS (don't interrupt)

### TASK-Q1 — Day 5 Retool Dashboard
- **Status:** in_progress
- **Detailed instructions:** [from-cowork/2026-05-18-4-day5-retool-dashboard.md](https://raw.githubusercontent.com/guyaronsky-777/non-est-handoff/main/from-cowork/2026-05-18-4-day5-retool-dashboard.md)
- **ETA:** 3–4h
- **Definition of done:** All 5 tile/panel sections live in Retool, screenshot reported to Yaron

---

## 🟧 P1 — IMMEDIATE NEXT (start as soon as Q1 finishes)

### TASK-Q2 — Wix Surgical Edits (DUAL POSITIONING) ⭐ HIGH LEVERAGE
- **Status:** queued — waiting for Q1
- **Detailed instructions:** [from-cowork/2026-05-18-7-deli-santa-maria-wix-quick-fixes.md](https://raw.githubusercontent.com/guyaronsky-777/non-est-handoff/main/from-cowork/2026-05-18-7-deli-santa-maria-wix-quick-fixes.md)
- **ETA:** ~30 min
- **Why it's urgent:** Yaron locked dual positioning — schema, title, meta description, and one body paragraph need to change before AI engines re-crawl. The schema fix is the single most impactful 30 minutes in the entire NON EST playbook.
- **Definition of done:** 4 edits live, schema validator passes, "Middle Eastern" + "Israeli" both confirmed in page source
- **Note:** File 7 has been REVISED for dual positioning (Middle Eastern leads, Israeli second). Read it fresh — do not use any prior memory of "Israeli-first everywhere."

### TASK-Q3 — Retry Reddit API App Creation 🔁
- **Status:** queued — Yaron's manual attempts failed silently (Reddit's developer portal bug or IP rate-limit). Chrome attempts on Yaron's behalf with his Reddit session.
- **Detailed instructions:** see "Reddit App — Chrome Attempt" section at the bottom of this file
- **ETA:** 5–15 min (depending on Reddit's mood)
- **Definition of done:** App created in https://www.reddit.com/prefs/apps; report back `client_id` + `client_secret` (paste directly to Yaron — he relays to Cowork via creds.txt). If Reddit still refuses, fall back to https://developers.reddit.com — same form, sometimes works when old portal doesn't.
- **Failure handling:** If both portals refuse, mark as 🔒 blocked and tell Yaron. We move to Plan C (paid Reddit data provider).

---

## 🟨 P2 — THIS WEEK (after immediate)

### TASK-Q4 — Day 6 Phase B: Wire Real Reddit Auth + Regenerate Action Queue
- **Status:** 🔒 blocked on Q3
- **Detailed instructions:** Phase B section of [from-cowork/2026-05-18-2-day6-action-queue-instructions.md](https://raw.githubusercontent.com/guyaronsky-777/non-est-handoff/main/from-cowork/2026-05-18-2-day6-action-queue-instructions.md)
- **ETA:** 30–60 min
- **Definition of done:**
  - n8n credential `Reddit OAuth (NON EST)` configured with real `client_id` + `client_secret`
  - Reddit Authority Scorer re-runs successfully for Deli Santa Maria
  - `reddit_authority` updates from placeholder (8) to real value (expected 10–30)
  - All 7 sub-scores in `reddit_signals` populated with real data
  - `reddit_action_queue` regenerates 3 fresh rows for Deli Santa Maria with real `thread_url` + `subreddit` values (no nulls)
  - Repeat for Ca Na Toneta to confirm cross-restaurant correctness

### TASK-Q5 — Mirror Report v3 Read + Familiarize
- **Status:** queued (Cowork pushes v3 shortly)
- **Detailed instructions:** Cowork will commit `from-cowork/2026-05-19-X-mirror-report-v3-and-template.md` once Sections 2/3/4 are strengthened with the dual positioning + the new 10 AI Signal Categories + Avatar Clarity Analysis
- **ETA:** 20 min read; deeper engagement during W21
- **Definition of done:** Chrome has read v3 and can reference its structure when generating Mirror sections for future restaurants

### TASK-Q6 — Sunday Review Prep (Day 7)
- **Status:** queued for Sat-Sun
- **Detailed instructions:** [from-cowork/2026-05-18-5-day7-sunday-review-template.md](https://raw.githubusercontent.com/guyaronsky-777/non-est-handoff/main/from-cowork/2026-05-18-5-day7-sunday-review-template.md)
- **ETA:** 1h
- **Definition of done:** Section 1 (Numbers) and Section 2 (Observations) of the template pre-filled by Chrome from Supabase data; sent to Yaron Sunday morning for the 60-min call

---

## 🟩 P3 — WEEK 21 (start after Sunday review, only if W21 is approved)

W21 theme (per Yaron 18 May 2026): **"AI Entity Engineering — Phase 1"**

### TASK-Q7 — Avatar Clarity Engine (build)
- **Status:** queued for W21 kickoff
- **Concept:** automated audit producing a per-identity-dimension Clarity table + AI Confusion Zones report. Reads schema, social, reviews, Reddit, AI engine outputs; outputs Weak/Medium/Strong per dimension.
- **Detailed instructions:** Cowork will write a W21 spec file before kickoff
- **ETA:** ~5 days

### TASK-Q8 — Mirror Report v3 Productization
- **Status:** queued for W21 kickoff
- **Concept:** automate what Cowork did manually for Deli Santa Maria — query the 4 AI engines via API, capture verbatim outputs, run Knowledge Audit + Visibility Mirror + Avatar Clarity Analysis + Dead Competitor Detection automatically. Output a Mirror Report PDF/markdown per restaurant.
- **ETA:** ~3–5 days
- **Dependencies:** OpenAI / Anthropic / Google / Perplexity API access (budget decision needed from Yaron)

### TASK-Q9 — Spain Ecosystem Map (config)
- **Status:** queued for W21 kickoff
- **Concept:** structured config file of 25–50 relevant Spain-specific channels (Tripadvisor, TheFork, Glovo, Just Eat, Uber Eats, Michelin, Verema, ElTenedor, Mallorca blogs, tourism portals, hotel concierge networks, Reddit subs, local FB groups, cycling/vegan/wedding communities). Avatar Clarity Engine reads this to know which channels to audit.
- **ETA:** ~2 days
- **Dependencies:** Yaron's domain knowledge of which channels matter most

### TASK-Q10 — Per-Segment Reddit Scoring (full implementation)
- **Status:** queued for W21
- **Concept:** Signal 7 (Competitor Benchmark) currently uses the flat competitor list; W20 partial. W21 finishes the per-segment iteration so each of Deli Santa Maria's 4 segments gets its own benchmark score + the overall is the segment average.
- **ETA:** ~1 day

### TASK-Q11 — Free-Audit Landing Page Hook ("Get your AI Mirror Report")
- **Status:** queued for W21
- **Concept:** simple landing page — restaurant owner enters name + city + email, gets an automated AI Mirror Report in 24h. Lead capture for the sales funnel. Reframes the "free audit" hook around the Mirror, not generic SEO.
- **ETA:** ~3 days
- **Dependencies:** Q7 + Q8 (the Mirror engine produces what this page promises)

### TASK-Q12 — Ca Na Toneta Reference Mirror Report
- **Status:** queued for W21 (after Q8 builds the engine)
- **Concept:** second reference Mirror, this time on Ca Na Toneta (the other test restaurant). Confirms the Mirror engine works for restaurants WITHOUT a strategic gap (standard-heuristic path).
- **ETA:** ~1 day

---

## 🟦 P4 — BACKLOG / MAINTENANCE

### TASK-Q13 — Drift Detection Workflow
- **Status:** backlog
- **Concept:** n8n daily job that detects when a restaurant's data online has changed (hours, menu, schema) vs the canonical Avatar; writes to `drift_alerts` table
- **ETA:** ~2 days

### TASK-Q14 — Add `requires_review` Column to `reddit_action_queue`
- **Status:** backlog (decision locked by Yaron — manager-reviewed always, never auto-execute)
- **ETA:** 30 min

### TASK-Q15 — Document the Bridge Architecture (README)
- **Status:** backlog
- **Concept:** lightweight README in the handoff repo explaining the Cowork ↔ Chrome ↔ Yaron flow, the folder structure, the file naming convention, the failure modes
- **ETA:** 30 min

### TASK-Q16 — Regenerate GitHub Token (clean cycle)
- **Status:** backlog — low priority. The token Yaron generated (and that was visible in a screenshot earlier) was already replaced once when his second regeneration invalidated it. The current token in use is the one shown in his second screenshot. If desired for cleanliness, regenerate once more.
- **ETA:** 5 min

---

## 🟪 P5 — FUTURE / W22+ (don't plan in detail yet)

- Agentic Component B (bookability — Reservation schema + TheFork/OpenTable detection)
- Agentic Component D (real agent simulation — now reframed as the Mirror engine's core)
- Avatar Engine — fully populate the 7-layer Canonical Avatar for Deli Santa Maria
- Distribution Engine — push the Avatar OUT to channels (today everything is read-only)
- Media Intelligence Engine — image classification, EXIF, fingerprinting
- Legacy Asset Recovery — review-response upgrader, media metadata enricher
- Temporal Intelligence — monthly snapshots, trend graphs
- More countries: Germany, UK, Israel ecosystem maps
- More clients: paying customer #1 (after the free-audit landing page produces leads)

---

## 🔁 Reddit App — Chrome Attempt (instructions for TASK-Q3)

**Background:** Yaron tried to create the Reddit app manually 3 times. Reddit's developer portal silently refused — likely captcha-timing issues, possibly Grammarly interference, possibly IP rate-limit. Chrome attempts now using Yaron's logged-in Reddit session.

### Pre-flight check

1. **Confirm Yaron is logged into Reddit** in his Chrome browser. Open https://www.reddit.com — verify "Welcome back, [username]" appears top-right. If not, ask Yaron to log in.

### Primary path: https://www.reddit.com/prefs/apps

1. Navigate to https://www.reddit.com/prefs/apps
2. Scroll to bottom. Click the link/button **"are you a developer? create an app..."**
3. Fill the form EXACTLY:
   - **name:** `NON EST Authority Scorer`
   - **type (radio):** select **`script`** (the third option — NOT "web app", NOT "installed app")
   - **description:** `Internal tool for NON EST`
   - **about url:** leave blank
   - **redirect uri:** `http://localhost:8080`
4. Solve the captcha if shown.
5. Click **"create app"** at the bottom.
6. If the form refreshes silently (the failure mode Yaron hit) → wait 30s, refresh the captcha, click "create app" once more (max 1 retry). If still failing → switch to Backup path.
7. If successful, you'll see a new app card with **"personal use script"** label. Capture:
   - **`client_id`** — short ~14-char string under "personal use script"
   - **`client_secret`** — longer string next to "secret" (click "edit" to reveal if hidden)

### Backup path: https://developers.reddit.com

If primary fails, try the newer portal at https://developers.reddit.com (Reddit's new developer interface — sometimes works when the legacy /prefs/apps page doesn't). Same form values. Capture the same two secrets.

### Reporting credentials back

**Do NOT paste the `client_secret` into chat with Cowork.** Tell Yaron:

> *"Reddit app created. client_id = [short_string_here]. client_secret is saved separately — Yaron, please paste both into a creds.txt inside Non Est folder and tell Cowork."*

Yaron will save them and Cowork picks them up via Read tool.

### If both portals refuse silently

Mark TASK-Q3 as 🔒 blocked. Report to Yaron with:
- What you tried
- What error (or non-error) appeared
- Recommendation: wait 24h and retry, OR move to Plan C (paid Reddit data provider — ScrapingBee, RapidAPI Reddit endpoints, ~$30–100/month)

---

## 📜 Changelog

| Date | Update |
|---|---|
| 2026-05-18 | Initial queue created. Q1–Q3 active; Q4–Q16 sequenced. |

---

*If you finish all P1+P2 tasks before this file is updated, stop and ping Yaron via WhatsApp — don't pull from P3 without explicit approval.*
