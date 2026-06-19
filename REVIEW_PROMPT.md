# REVIEW PROMPT — paste this to Claude inside a project to act on findings

Use this when you want to go through what the routine found and decide what to do.
Copy everything below the line and send it to Claude while working inside the project.

---

Help me review and act on this project's radar findings.

1. Read this project's `RADAR_FINDINGS.md` (the daily auto-report) and `RADAR_DECISIONS.md`
   (my decisions record). If the decisions file doesn't exist, create it with sections
   Adopted / Try later / Skipped.

2. From the findings, take the real opportunities (scored 80+). Remove duplicates, and drop anything
   already listed anywhere in the decisions file (already adopted, try-later, or skipped).

3. Show me a short shortlist of what's left. For each: name, what it does, why it fits us (now or next),
   and your recommendation — **Adopt now / Try later / Skip**.

4. I'll decide each. For "Adopt now," help me actually set it up.

5. Record every decision in `RADAR_DECISIONS.md` (date, item, reason). Put skips under
   "Skipped (do not re-suggest)" so they don't come back.

Keep it short and plain. Do NOT edit `RADAR_FINDINGS.md` — the routine owns that.
