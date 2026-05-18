# NON EST — Handoff Bridge

**What this repo is:** A shared, version-controlled space for two AI assistants to exchange instructions and reports without Yaron having to copy-paste between chats.

- **Claude in Cowork** (filesystem + sandbox access) writes to `from-cowork/`
- **Claude in Chrome** (browser + Supabase/n8n/Retool access) reads from `from-cowork/`, writes reports to `from-chrome/`
- Yaron stays in the loop — he reviews handoffs and triggers each side

---

## Folder structure

| Folder | Who writes here | Who reads here |
|---|---|---|
| `from-cowork/` | Claude in Cowork | Claude in Chrome (and Yaron) |
| `from-chrome/` | Claude in Chrome (or Yaron pasting reports) | Claude in Cowork (and Yaron) |
| `archive/` | Either | Reference — past handoffs moved here after 30 days |

---

## Handoff file naming convention

`YYYY-MM-DD-N-topic.md` — e.g. `2026-05-18-1-day2-reddit-scorer-instructions.md`

The `N` is a counter for multiple handoffs on the same day.

---

## Each handoff file must have

```markdown
# Title — one line summary

**From:** Claude in Cowork (or Claude in Chrome)
**To:** Claude in Chrome (or Claude in Cowork)
**Date:** YYYY-MM-DD HH:MM CET
**Status:** awaiting / in_progress / done / blocked
**Subject:** one-line topic

---

## Context
What this is about, why it's being sent now.

## Instructions / Findings
The actual content.

## Expected response
What the receiver should do, and where to put the reply (usually a file in the other folder).
```

---

## How Yaron triggers a handoff

1. **From Cowork → Chrome:** Yaron tells Cowork what he wants. Cowork writes a file in `from-cowork/`, pushes to GitHub. Yaron tells Chrome: *"Read the latest in our handoff repo from-cowork folder."*
2. **From Chrome → Cowork:** Chrome writes a report to `from-chrome/` (via GitHub web UI or by Yaron pasting). When Yaron returns to Cowork, he says: *"Check our handoff repo from-chrome folder."*

---

## Repository URL

`https://github.com/guyaronsky-777/non-est-handoff`

Latest from-cowork files (web view, auto-renders markdown):
`https://github.com/guyaronsky-777/non-est-handoff/tree/main/from-cowork`

---

*Bridge initialized 18 May 2026 by Claude in Cowork.*
