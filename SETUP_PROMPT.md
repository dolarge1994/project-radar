# SETUP PROMPT — paste this to Claude inside the project you want to monitor

This is the cold-start instruction. Copy everything below the line and send it to Claude (Claude Code)
while working inside the project. You don't need any prior context.

---

You are setting up a "project-radar" routine for THIS project — a daily scheduled task that watches a
fast-moving field and tells me only what fits this project now and next. Use my reusable template.

1. Read these files from the project-radar repo (or the folder I gave you):
   - `SKILL.template.md`
   - `NOTIFICATIONS.md`

2. Learn THIS project from its own docs. Find the 3–5 files that best describe it as a whole,
   INCLUDING its roadmap / future plan. List them and confirm with me.

3. Make a filled-in copy of `SKILL.template.md`, replacing every `{{PLACEHOLDER}}`:
   - `{{PROJECT}}` — the project's name
   - `{{WHAT_THIS_WATCHES}}` — what to track (default: new AI dev tools / Claude Code skills / MCP servers;
     or anything else — papers, CVEs, competitors, regulations…)
   - `{{TASK_ID}}` — kebab-case id, e.g. `<project>-radar`
   - `{{CONTEXT_FILES}}` — the key files from step 2 (absolute paths); use the same list in Steps 1 and 7
   - `{{FIXED_FACTS}}` — a few always-true facts about the project (stack, platform, who it's for)
   - `{{SCAN_SOURCES}}` — where to look (keep/edit the defaults to match `{{WHAT_THIS_WATCHES}}`)
   - `{{PROJECT_FINDINGS_FILE}}` — absolute path to `RADAR_FINDINGS.md` at the project root
   - `{{PROJECT_DECISIONS_FILE}}` — absolute path to `RADAR_DECISIONS.md` at the project root
   Scoring is by real fit (built / now / next), NOT by how new or popular something is.

4. Ask me what hours I'm usually online, and create the scheduled task with a schedule that fits — e.g.
   `0 9,13,17 * * *` (9am/1pm/5pm). It runs once a day; the extra times just skip; a missed day catches up
   on next launch. (See the README "Schedule" cheat-sheet.)

5. Set up the working files and keep them local-only: the routine creates `RADAR_FINDINGS.md` on first run;
   create `RADAR_DECISIONS.md` (Adopt / Try-later / Skip) at the project root; add BOTH to the repo's
   `.git/info/exclude`.

6. Tell me to do what you can't from a file: (a) add my notification secret (email app password OR a
   Discord/Slack webhook URL — see NOTIFICATIONS.md) to the LIVE task file only; (b) set the task's model
   to a fast/cheap one in the Edit screen. Then I'll click Run now once to approve permissions.

7. Confirm the built-in behavior: once/day with catch-up, reads several files incl. the roadmap, current-year
   searches, fit-based scoring, decision-aware (never re-alerts something I've decided on), clean in-project
   report, ONE daily digest of the 80+ matches.

To act on findings later, use `REVIEW_PROMPT.md`. Keep your questions to me minimal and in plain language.
