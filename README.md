# Mission OS

A personal life operating system — task management, goal tracking, calendar, and AI-powered productivity, all in a single self-contained HTML file.

---

## What It Is

Mission OS is a mobile-first web app you host yourself. It combines Claude AI with Google Sheets as a database so your tasks, goals, and calendar events sync across all your devices without a monthly subscription or app store.

---

## Features

### Dashboard
- Priority queue ranked by urgency × importance (scored by Claude)
- Quick-add text box — type a task, Claude validates and chunks it
- Inline analysis panel — review Claude's recommendation before committing
- Filter by category: Work, Personal, Health, Interview Prep
- Stats strip: total tasks, planned hours, completion %

### Capture
- **Voice input** — tap mic, speak your task (Chrome on Android natively supported)
- **Text / email paste** — paste any text, Claude extracts and structures the task
- Full Claude analysis: urgency score, importance score, priority score, suggested time blocks, chunked breakdown

### Calendar
- Interactive day/week view (6 AM – 10 PM)
- Click or drag any empty slot to add a task
- Blocked slots from your Outlook screenshot displayed as grey busy blocks
- Tasks appear as color-coded blocks at their scheduled time
- Date navigation with ‹ › arrows
- **Sync to Google Calendar** — pushes non-work tasks directly into Google Calendar

### Goals
- Input a high-level goal + description of current progress
- Claude breaks it into 3–6 sequential milestones, each with 2–5 actionable tasks
- Progress percentage calculated from completed tasks
- Check off individual tasks — progress bars update in real time
- **→ Queue** button on each milestone sends pending tasks to your priority queue

### Backlog
- Frictionless parking lot for ideas — no deadline, no pressure
- Claude analyzes backlog items on demand
- **→ Queue** promotes any item to the priority queue when you're ready

### Timeline
- Visual day view of your scheduled tasks
- Export work tasks as `.ics` for Outlook import

### Security
- **AES-256-GCM encryption** via Web Crypto API
- Your Claude API key and Google Sheets URL are encrypted behind a PIN (mobile) or password (desktop)
- Keys stored in `localStorage` — never transmitted anywhere except to Anthropic's API and your own Google Sheet
- Lock button clears all keys from memory instantly

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML/CSS/JS — zero dependencies, zero build step |
| AI | Claude API (Sonnet 4.6 by default) |
| Database | Google Sheets via Google Apps Script |
| Auth | AES-256-GCM encryption (Web Crypto API) |
| Hosting | GitHub Pages / Netlify (any static host) |
| Calendar sync | Google Calendar deep link API |
| Offline | Service Worker (PWA-ready) |

---

## Setup

### 1. Host the app

Upload `mission-os.html` as `index.html` to GitHub Pages or drag-and-drop to [Netlify Drop](https://app.netlify.com/drop).

**GitHub Pages:**
```
Repo → Settings → Pages → Deploy from branch (main, / root)
URL: https://yourusername.github.io/your-repo-name
```

### 2. Set up the database (Google Sheets)

1. Go to [script.google.com](https://script.google.com) → **New project**
2. Delete existing code and paste the contents of `mission-os-gas.js`
3. Click **Deploy → New deployment**
   - Type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Copy the **Web App URL**
5. Test it: `YOUR_URL?action=ping` should return `{"ok":true}`

### 3. Get your Claude API key

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create an API key
3. Estimated cost: ~$10–20/month at 1,000 analyses using Claude Sonnet 4.6

### 4. First-time app setup

1. Open the hosted URL
2. You'll see a PIN/password setup screen
3. Choose a 4-digit PIN (mobile) — the same PIN works as a password on desktop
4. Enter your Claude API key and Google Sheets URL
5. Tap **🔐 Encrypt & Save**

Your keys are AES-256 encrypted and stored in `localStorage`. You only ever enter them once per device.

---

## Usage

### Adding tasks
| Method | How |
|---|---|
| Quick-add | Type in the dashboard text box → Enter |
| Voice | Capture tab → tap mic → speak |
| Email paste | Capture tab → paste email body |
| Calendar | Calendar tab → click/drag a time slot |
| Goal breakdown | Goals tab → Claude generates tasks automatically |

### Completing tasks
- **✓ Complete Chunk** — steps through chunks one at a time
- **✓✓ All Done** — marks the whole task complete instantly
- **↩ Reopen** — reopens a completed task

### Syncing to your work calendar
Work tasks: **Task detail → Outlook ICS** — imports directly into Outlook.

Personal tasks: **Calendar tab → Sync to Google Calendar** — opens each event in Google Calendar pre-filled.

### Uploading your Outlook calendar
Dashboard → **Upload Outlook Calendar Screenshot** → Claude parses blocked/free slots → calendar view updates automatically.

---

## File Structure

```
mission-os.html      # The entire app — self-contained
mission-os-gas.js    # Google Apps Script backend (paste into script.google.com)
README.md            # This file
```

---

## Cost

| Service | Cost |
|---|---|
| GitHub Pages | Free |
| Google Sheets + Apps Script | Free |
| Claude API (Sonnet 4.6) | ~$10–20/month at typical usage |
| Google Calendar sync | Free |

---

## Security Notes

- Your Claude API key and Google Sheets URL are **never stored in plaintext**
- Encryption uses AES-256-GCM with PBKDF2 key derivation (200,000 iterations)
- All API calls go directly from your browser to Anthropic — nothing proxied
- The Google Sheet is protected only by the Apps Script URL — keep it private
- Goal data is stored in `localStorage` only (not synced to Sheets)

---

## Roadmap

- [ ] Google Calendar read API (pull real events instead of screenshot parsing)
- [ ] Firestore sync option (real-time instead of 30s polling)
- [ ] Push notifications for task time blocks
- [ ] Capacitor wrapper for a native Android APK
- [ ] Multi-user support with Google SSO
- [ ] Weekly review AI summary

---

## Built With

Designed and built iteratively with Claude (Anthropic) as a personal productivity tool for a Senior Amazon Program Manager. All AI features use the Claude Sonnet 4.6 model via the Anthropic Messages API.
