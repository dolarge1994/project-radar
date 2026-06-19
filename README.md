# project-radar

**A self-hosted [Claude Code](https://www.anthropic.com/claude-code) routine that watches a fast-moving field and tells you only what actually fits *your* project — right now and next.**

## The problem

The space moves faster than anyone can follow — and you're **heads-down building**, not refreshing feeds. Two things quietly eat your time:

1. **Hunting** for what's new.
2. Even when you spot something, working out **"does this actually fit what I'm building right now?"**

project-radar does both for you — so your time goes to **adopting the right things**, not searching for them or second-guessing whether they fit.

---

Newsletters and tool directories tell you what's new *in general*. project-radar is different: it reads **your project's own notes and roadmap**, searches the sources you care about, and scores each find by **how much it helps what you're building now and what's coming next** — then sends you a short daily digest of just the strong matches. Everything it finds is written back into your project, so your AI assistant can review it with you and you decide what to adopt.

> **Default example:** watches the **AI-tooling space** (new Claude Code skills, MCP servers, dev tools).
> But it's a general engine — point it at research papers, security advisories, competitor releases, regulations, grants… anything that moves faster than you can follow.

## Why it's different

- **Personalized to your project, not to keywords.** It reads your project files and judges fit to your current priorities *and* your roadmap (what you've **built**, what you're doing **now**, what's **next**).
- **Low noise.** One digest a day, only things scoring 80+. Silence when nothing's worth your time.
- **It remembers your decisions.** Skip something once and it never nags you again.
- **Your AI stays in the loop.** Findings land *inside* the project, so you can say "review today's findings" and decide together.
- **Self-monitoring.** An optional weekly health check confirms it's still running.

## How it works (each day)

1. Reads several of your project's context/roadmap files.
2. Searches the sources you configured (web, GitHub, registries, …) using the current year.
3. Scores each find 0–100 by real fit to your project (now + next) — not by hype.
4. Sends **one** digest of the 80+ matches (email, or Discord/Slack).
5. Writes a clean, de-duplicated report into your project (`RADAR_FINDINGS.md`).
6. You review it with your AI assistant and track **Adopt / Try-later / Skip** decisions (`RADAR_DECISIONS.md`).

## Requirements

- **Claude Code (desktop)** with **scheduled tasks** — this is the runtime that executes the routine.
- One way to reach you: an **email** account (SMTP app password) **or** a **Discord/Slack webhook**. See [NOTIFICATIONS.md](NOTIFICATIONS.md).
- Works on **Windows, macOS, Linux** (state lives under `~/.claude/...`).

## Quick start

1. Open Claude Code **inside the project** you want to monitor.
2. Paste [`SETUP_PROMPT.md`](SETUP_PROMPT.md) to Claude — it reads the template, learns your project, fills in the blanks, and creates the scheduled task.
3. Pick a **schedule** that suits your hours (see below), choose a fast/cheap **model**, and add your notification secret to the **live** task file (never commit it — see [NOTIFICATIONS.md](NOTIFICATIONS.md)).
4. Optional: add the [`health-check.template.md`](health-check.template.md) for a weekly "still running" ping.

A full worked example is in [`examples/`](examples/).

## Schedule (when it runs)

You choose when the routine fires with a cron expression. It does real work **once a day** (the extra times just check and skip), so you only need a few times during *your* active hours:

| Cron | Plain English |
|------|---------------|
| `0 9,13,17 * * *` | 9am, 1pm, 5pm every day — a solid default |
| `0 7 * * *` | 7am every day |
| `0 12,16,20 * * *` | noon, 4pm, 8pm — night owl |
| `0 9-18 * * *` | every hour, 9am–6pm — maximum safety |

Pick times you're usually at your machine. If it's off all day, it **catches up** the next time you open the app — you won't miss a day. (The optional health check works the same way: it checks daily but emails a status only about once a week, so you can't miss it by being offline on a particular day.)

## Adapt to any topic

Swap the `SCAN_SOURCES` (where to look) and the domain hint in `SCORING` in [`SKILL.template.md`](SKILL.template.md). The engine — *read project → scan → score by fit → digest → decide* — stays the same whatever you're tracking.

**Example — track security advisories instead of AI tools:** set `{{WHAT_THIS_WATCHES}}` to *"new CVEs and advisories affecting our dependencies"* and point `SCAN_SOURCES` at the NVD feed, GitHub Security Advisories, and your ecosystem's advisory database. It then scores each advisory by how much it touches *your* stack.

## Tuning

- **Too many / too few alerts?** Raise or lower the `80` cutoff in Step 5 of the template.
- **Different cadence or hours?** Change the schedule (see above).
- **Different model?** Set it in the task's Edit screen — a fast/cheap model is plenty.

## Files

| File | What it is |
|------|-----------|
| `SKILL.template.md` | The routine itself (fill in the `{{PLACEHOLDERS}}`) |
| `SETUP_PROMPT.md` | Paste to Claude to set it up for a project |
| `REVIEW_PROMPT.md` | Paste to Claude to review findings and decide |
| `health-check.template.md` | Optional weekly "is it still running?" heartbeat |
| `NOTIFICATIONS.md` | Email / Discord / Slack + Windows/macOS/Linux setup |
| `examples/` | A worked example + sample output |

It creates two files **inside your project** (kept local via `.git/info/exclude`): `RADAR_FINDINGS.md` (the auto report) and `RADAR_DECISIONS.md` (your adopt/skip record).

## Safety / disclaimer

This routine runs web searches/fetches and sends messages using **your** credentials. **Read the template before you run it.** Keep secrets (email passwords, webhook URLs) only in the live task file — never in git. Provided **as-is, with no warranty** (see [LICENSE](LICENSE)).

## Status

Shared **as-is** under MIT — a working tool, not a managed product. Issues and PRs are welcome but may not get a response. Fork it freely and make it yours.

## License

[MIT](LICENSE).
