---
name: networking-tracker
description: Set up and update a personal LinkedIn networking tracker artifact (connection requests, statuses, follow-up dates). Use when the user says connection sent, accepted, followed up, or asks to start a networking tracker.
---

# Networking Tracker

Keeps a personal tracker page for LinkedIn connection requests. The page is one self-contained HTML file. All data lives in one JS array, so every update is a small scripted text edit. Never retype or print the page.

## First-time setup (user has no tracker yet)
1. Check whether the user already has one: Artifact `list` and look for a page titled "Networking Tracker". If found, use that URL and skip to Updating.
2. Otherwise the user should attach `template/index.html` from the repo. Use that attached file. Do not rewrite it from memory.
3. Publish a copy with Artifact `publish` and no `url`, so the user gets their own private tracker. Tell the user in one sentence that it is ready. Remember the URL for this conversation.
If no template file is attached, ask the user to attach `template/index.html` from the repo.

## Updating (token rules)
- Batch every change from the user's message into ONE python script and ONE publish.
- First look for the file on disk: Glob `**/artifact-files/*/index.html`. If missing, Artifact `read` the user's tracker URL with `path: "index.html"` once.
- Never re-read to verify, never screenshot, never change styles or layout.
- Reply with one or two sentences saying what changed. Do not paste the URL.
- If publish is refused because a newer version exists, re-read with `path`, re-run the same script (guard against duplicates), and publish again.

## Row format
Rows go in `const conns = [ ... ];`. Insert before the `];` that closes the `conns` array (the text `];\n\nconst statusMeta` marks it).
```
  { person: "Name", role: "Title", company: "Company", status: "pending", sent: "2026-01-15", notes: "Mutual connections, which note was used, follow-up date." },
```
- `sent` must be YYYY-MM-DD (use today's date unless the user says otherwise).
- `status` is one of: pending, accepted, followed, noresp.
- Do not use double quotes inside notes.
- Add exactly what the user stated. Do not invent people, dates or outcomes. If unsure whether a request was sent, mark it pending.
- Check that the person is not already in the array before inserting; update the existing row instead.

## Changing a status
```python
import re
s = re.sub(r'(person: "NAME".*?status: ")\w+(")', r'\1accepted\2', s, count=1, flags=re.S)
```
When someone accepts or replies, change the status and append the date to their notes.

## Follow-up rhythm (default)
- Pending after 3 to 5 days: send a short follow-up if they accepted; email if not accepted (the page flags pending rows as due after 4 days).
- Followed up with no reply after another 3 to 5 days: mark no response and move on.

## Connect notes
When the user asks for a note, follow `guides/connect-notes.md` if available; otherwise apply these rules:
- Stay under 300 characters and count them.
- Start with a first name, then one line on who you are.
- Add one specific, true hook about the person (their path, team or work).
- Ask for one low-pressure thing: to connect and learn. Do not ask for a job, referral or resume in the first message.
- Keep visa status and start dates out of the first message.
- Name only skills you actually have. Put a portfolio link last, with https.
