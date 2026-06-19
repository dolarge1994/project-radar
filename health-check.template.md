---
name: project-radar-health-check
description: Health check for project-radar routines — sends a short status about once a week showing which routines ran recently, and flags any that haven't. Auto-covers every routine.
---

# === project-radar HEALTH CHECK (heartbeat) ===
# ONE of these covers ALL your project-radar routines — it auto-discovers them, so no per-project edits.
#
# SCHEDULE: run it DAILY, e.g.  0 20 * * *  (8pm). A built-in once-per-week gate means it only actually
#   SENDS about once every 7 days — on the first day you're online after a week passes. So it does NOT
#   depend on you being online at any specific day/time; you can't miss it by being off on (say) Sunday.
#   Catch-up on launch also applies if the app was closed.
#
# NOTIFY: email by default; for Discord/Slack, swap the send block per NOTIFICATIONS.md. Secret in the LIVE file only.

You are the health monitor for the project-radar routines. Your ONLY job: about once a week, check each routine has run recently and send a short status. Run the Python below with Bash, then stop. Do nothing else.

```python
import glob, json, os, datetime, smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

base = os.path.join(os.path.expanduser("~"), ".claude", "scheduled-tasks")
today = datetime.date.today()

# --- Once-per-week gate: only send if it's been >= 7 days since the last status (or first run). ---
state_file = os.path.join(base, "project-radar-health-check", "health-state.json")
last_sent = ""
if os.path.exists(state_file):
    try:
        last_sent = json.load(open(state_file, encoding="utf-8")).get("last_sent_date", "")
    except Exception:
        last_sent = ""
if last_sent:
    try:
        if (today - datetime.date.fromisoformat(last_sent)).days < 7:
            print("SKIP: a status was sent within the last 7 days")
            raise SystemExit
    except SystemExit:
        raise
    except Exception:
        pass

# --- Auto-discover every routine that has a project-radar state file, and check freshness. ---
status_files = sorted(glob.glob(os.path.join(base, "*", "sent-findings.json")))
rows, problems = [], 0
for fp in status_files:
    name = os.path.basename(os.path.dirname(fp))
    last = ""
    try:
        last = json.load(open(fp, encoding="utf-8")).get("last_run_date", "")
    except Exception:
        last = ""
    days = (today - datetime.date.fromisoformat(last)).days if last else 999
    ok = days <= 3
    if not ok:
        problems += 1
    rows.append(f"[{'OK' if ok else '!!'}] {name}: last ran {last or 'never'}" + (f" ({days}d ago)" if last else ""))

if not status_files:
    rows = ["No project-radar routines found under scheduled-tasks."]
    problems = 1

sender = "you@example.com"               # set in the LIVE file only
password = "PUT_YOUR_APP_PASSWORD_HERE"  # set in the LIVE file only; never commit
recipient = "you@example.com"
status = "ALL OK" if problems == 0 else f"{problems} need a look"
subject = f"Radar health — {status} — {today.isoformat()}"
body = ("Weekly project-radar health check\n\n" + "\n".join(rows)
        + "\n\nOK = ran within the last 3 days.\n"
          "'!!' = hasn't run recently: either the task is failing (open it / Run now) "
          "or your machine was simply off those days (then ignore).")

# --- EMAIL (default). For Discord/Slack, replace this block per NOTIFICATIONS.md, reusing `body`. ---
msg = MIMEMultipart(); msg['From'] = sender; msg['To'] = recipient; msg['Subject'] = subject
msg.attach(MIMEText(body, 'plain'))
with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
    server.login(sender, password)
    server.send_message(msg)

# --- Record that we sent today, so the weekly gate works. ---
os.makedirs(os.path.dirname(state_file), exist_ok=True)
json.dump({"last_sent_date": today.isoformat()}, open(state_file, "w", encoding="utf-8"))
print("Health email sent:", subject)
```

Runs daily, sends about once a week. Do nothing else.
