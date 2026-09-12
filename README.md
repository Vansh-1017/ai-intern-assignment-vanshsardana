# AI Intern Assignment — HSA Team

**Name:** Vansh Sardana

**Email:** vanshsardana874@gmail.com

**Date:** September 12, 2026

## Part A — Lead Capture Form

Pure HTML/CSS/vanilla JS form (`part-a/index.html`) with inline validation, a 300-char live counter, and a no-reload thank-you screen. Logs the submitted JSON to the browser console.

**To run:** open `part-a/index.html` directly in any browser (double-click it), or serve it locally with `python3 -m http.server 8000` and visit `http://localhost:8000`.

## Part B — n8n Workflows

- **`workflow-b1-lead-notification.json`** — Webhook → field extraction → IF (Postgraduate/PhD) → Gmail notification, else → Google Sheets row.
- **`workflow-b2-scheduled-fetch.json`** — Daily 9AM schedule → HTTP Request → Code (filter/transform) → Set → Gmail digest.

**API used in B2:** [Hipolabs Universities API](http://universities.hipolabs.com) — free, no key required, and thematically fits Part A's "preferred university" field. Fetches the first 10 US universities daily and emails a formatted digest.

**To run:** import both JSON files into n8n (⋮ menu → Import from File), reattach your own Gmail/Google Sheets credentials, and activate both workflows.

**Credential placeholders to replace:**
| Placeholder | Where | Replace with |
|---|---|---|
| `PLACEHOLDER_GMAIL_CRED_ID` | Both Gmail nodes | Your Gmail OAuth2 credential |
| `PLACEHOLDER_GOOGLE_SHEETS_CRED_ID` | B1 Sheets node | Your Google Sheets OAuth2 credential |
| `PLACEHOLDER_GOOGLE_SHEET_ID` | B1 Sheets node | Your actual Sheet ID |
| `notifications@hsateam.example` | Both Gmail `sendTo` fields | Your real notification email |
| Sheet tab `"Leads"` | B1 Sheets node | Your sheet's tab name (columns: Name, Email, Course Level, Preferred University, Country, Message, Timestamp) |

## Part C — Integration (Bonus)

`part-c/index.html` extends Part A with a `fetch()` POST to the B1 webhook, a loading spinner on submit, and error/success banners.

**To run:** replace `WEBHOOK_URL` in the script with your B1 Production webhook URL, then open the file the same way as Part A.

**Demo recording:** `part-c/drive-link.txt` or https://drive.google.com/file/d/1nzCzCd8ZeLGhVnBb5oUohaGvgxwLw5b8/view?usp=sharing


## Challenges Faced

The Code node in B2 initially assumed the HTTP Request node returned one item wrapping the full array, but n8n auto-splits JSON array responses into one item per element — this caused null fields until the code was rewritten to read `$input.all()` directly. Separately, the Gmail digest node ran once per item instead of once per batch, sending 10 duplicate emails; fixing this required enabling "Execute Once" on that node.
