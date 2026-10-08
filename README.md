# Claude Networking Tracker

A private tracker for LinkedIn connection requests, built to be updated by Claude in one line. It has a blank tracker page, a Claude skill that updates it, and a short guide to writing connect notes.

Nothing is sent or automated. You send requests on LinkedIn yourself; this keeps track of them.

## What you get

- **Tracker page** (`template/index.html`): connection requests with status (Pending, Accepted, Followed up, No response), sent date, notes, filter chips, and a "Follow up now" flag when a pending request is 4 days old.
- **Skill** (`skill/networking-tracker/SKILL.md`): lets Claude add and update rows in one small edit, with few tokens.
- **Guide** (`guides/connect-notes.md`): a connect-note template, best practices, and follow-up timing.

## Setup (about 2 minutes)

1. Download this repo (Code, then Download ZIP) and unzip it.
2. Add the skill to Claude: upload `skill/networking-tracker/SKILL.md` in Settings, then Skills.
3. Start a chat, attach `template/index.html`, and say: **"start my networking tracker"**. Claude publishes a private copy of the page as your own tracker.

You need a Claude account that supports skills and artifacts.

## Daily use

Tell Claude what happened, in plain words:

- "Sent a request to Alex Rivera, Data Engineer at Acme. Note was about her path from my school."
- "Alex accepted."
- "Followed up with Alex."
- "Who is due for follow-up?"

Claude updates the row and republishes. Open the tracker any time to filter and review.

The status dropdown on each row also works, but it is saved in your browser only. Updates made through Claude are what persist.

## Privacy

- Your tracker is private to you unless you share the link.
- This repo contains no personal data. Keep your real tracker out of any public place, and use fake rows in any screenshot you share.

## How it works

All data lives in one JS array (`const conns`) in the page. The skill edits that array with a small script and republishes, so updates stay cheap and the layout never changes.

## License

MIT. See `LICENSE`.
