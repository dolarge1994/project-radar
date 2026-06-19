# Example: "Acme Notes" (a fictional React note-taking app)

This shows the placeholder values you'd drop into `SKILL.template.md` for a real project. Acme Notes is a
small web app (React + Vite, Supabase backend) whose team wants to track new AI dev tools relevant to it.

```text
{{PROJECT}}             = Acme Notes
{{WHAT_THIS_WATCHES}}   = new AI dev tools, Claude Code skills, and MCP servers useful for a React/Supabase web app
{{TASK_ID}}             = acme-notes-radar
{{CONTEXT_FILES}}       =
    /home/dev/acme-notes/docs/OVERVIEW.md
    /home/dev/acme-notes/docs/CURRENT_SPRINT.md
    /home/dev/acme-notes/docs/ROADMAP.md
{{FIXED_FACTS}}         = React 18 + Vite; Supabase (Postgres/auth); Vercel hosting; small team; built with Claude Code; macOS
{{PROJECT_FINDINGS_FILE}}   = /home/dev/acme-notes/RADAR_FINDINGS.md
{{PROJECT_DECISIONS_FILE}}  = /home/dev/acme-notes/RADAR_DECISIONS.md
```

For `{{SCAN_SOURCES}}`, keep the default AI-tooling sources and add one project-specific search:

```
- Search "React Supabase AI tooling <year> site:github.com"
```

Everything else in `SKILL.template.md` stays as-is. Then:

1. Create the scheduled task with the filled prompt, id `acme-notes-radar`. Pick a schedule for your hours —
   the Acme team works 9–6, so they use `0 9,13,17 * * *` (9am/1pm/5pm; runs once a day).
2. Set a fast/cheap model in the task's Edit screen.
3. Put your email app password (or Discord/Slack webhook) into the **live** task file only.
4. Create `/home/dev/acme-notes/RADAR_DECISIONS.md` and add both `RADAR_*.md` files to
   `/home/dev/acme-notes/.git/info/exclude`.
5. `Run now` once to approve permissions.

See `sample-output.md` for what the daily digest and the in-project report look like.
