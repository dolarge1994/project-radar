# Notifications & platform setup

The routine needs one way to reach you. Pick **email** or a **webhook** (Discord/Slack), and put the
secret only in the **live** task file — never in git.

## Option A — Email (SMTP)

Set `sender`, `password`, `recipient` in the task's Step 6 (and in the health check).

- **Gmail:** make an **App Password** (Google Account → Security → 2-Step Verification → App passwords).
  Use that 16-character password, not your normal one. Host `smtp.gmail.com`, port `465` (SSL) — already in the template.
- **Outlook/Microsoft 365:** `smtp.office365.com`, port `587` (STARTTLS) — use `smtplib.SMTP(...)` + `server.starttls()` instead of `SMTP_SSL`.
- **Fastmail / others:** use their SMTP host/port and an app-specific password.

## Option B — Discord webhook

Discord → Server Settings → Integrations → Webhooks → **New Webhook** → Copy URL. Replace the send block with:

```python
import urllib.request, json
url = "<YOUR_DISCORD_WEBHOOK_URL>"
payload = json.dumps({"content": body[:1900]}).encode()   # Discord caps messages near 2000 chars
urllib.request.urlopen(urllib.request.Request(url, data=payload, headers={"Content-Type": "application/json"}))
```

## Option C — Slack webhook

Slack → create an **Incoming Webhook** → Copy URL. Replace the send block with:

```python
import urllib.request, json
url = "<YOUR_SLACK_WEBHOOK_URL>"
payload = json.dumps({"text": body}).encode()
urllib.request.urlopen(urllib.request.Request(url, data=payload, headers={"Content-Type": "application/json"}))
```

> Tip: build the same `body` string the template already builds, then send it via whichever channel you chose.

## Keeping secrets out of git

The live task file lives at `~/.claude/scheduled-tasks/<task-id>/SKILL.md`, which is **outside** your project
repo — so the secret there is never committed. If you put any generated file (report/decisions) inside your
project, add its filename to the repo's `.git/info/exclude` (local ignore, no commit needed).

## Platforms / paths

State is stored under `~/.claude/scheduled-tasks/<task-id>/`. The template uses `os.path.expanduser("~")`,
so it resolves correctly everywhere:

- **Windows:** `C:\Users\<you>\.claude\scheduled-tasks\...`
- **macOS:** `/Users/<you>/.claude/scheduled-tasks/...`
- **Linux:** `/home/<you>/.claude/scheduled-tasks/...`

Your project's context files are absolute paths you provide when filling in the template.
