# 🛡️ Apex Sentry v4.0

> **Real-time AI-powered child safety monitoring for gaming and streaming platforms**

Apex Sentry is a single-file HTML application that monitors Twitch, Discord, Kick, YouTube, Roblox, and 8 more platforms for grooming behavior in real time. It uses pattern matching, behavioral sequence detection, and Claude AI to identify, flag, and document predatory behavior — giving investigators, parents, and child safety professionals a powerful tool that requires zero infrastructure to run.

-----

## ⚡ Quick Deploy

**Option 1 — Netlify Drop (30 seconds)**

1. Download `apex-sentry-full.html`
1. Go to [netlify.com/drop](https://netlify.com/drop)
1. Drag the file onto the page
1. Live URL appears instantly

**Option 2 — GitHub + Netlify (auto-deploy on every push)**

1. Fork this repo
1. Connect to Netlify via **Import from GitHub**
1. Every `git push` triggers an automatic redeploy

**Option 3 — Run locally**

```bash
# No build step. Just open the file.
open apex-sentry-full.html
```

> ⚠️ Running locally limits some features (CORS blocks Twilio SMS and certain API calls). Deploy to Netlify for full functionality.

-----

## 🎯 What It Does

Apex Sentry monitors chat streams in real time and scores every message through a 3-tier detection engine:

|Flag Level           |Risk Score|Trigger                                            |Action                                                          |
|---------------------|----------|---------------------------------------------------|----------------------------------------------------------------|
|**Flag-1** (Monitor) |0–30      |Passive indicators                                 |Log only                                                        |
|**Flag-2** (Elevated)|31–60     |Contact requests, flattery, off-platform migration |Parent dashboard alert                                          |
|**Flag-3** (Critical)|61–100    |Explicit grooming, image requests, meeting attempts|Immediate intervention, evidence preservation, push notification|

-----

## 📋 Features

### 🔴 Live Monitor

- **Multi-channel dashboard** — monitor up to 4 channels simultaneously (xqc, shroud, ninja, pokimane — or any channel)
- **Platform switcher** — Twitch, Discord, Kick, YouTube — interchangeable in one click
- **Live word cloud** — top words/phrases color-coded by risk level, updates every 3 seconds
- **Chat speed metrics** — msgs/min, flag rate %, unique users, peak speed with sparkline graph
- **Grooming sequence detector** — tracks multi-message escalation patterns
- **Simulation mode** — falls back to realistic demo data if live connection fails

### 🤖 Discord Bot Installer

- One-click bot connection via Bot Token
- Monitors all server channels through Discord Gateway WebSocket
- Invite link generator with correct permission scopes
- 6 behavior toggles: auto-delete F3, timeout on F3, DM server owner, log channel, scan DMs, voice join alerts
- Live terminal with commands: `status`, `profiles`, `patterns`, `cases`, `clear`

### 📊 Real-Time Analytics

- Live KPI grid (Critical, Elevated, Scanned, Threat Rate, Suspect Profiles, Evidence Packages)
- SVG threat timeline sparkline chart
- Platform breakdown bars
- Hourly heatmap — based on real NCMEC predator activity data (peaks 3pm–midnight)
- Top triggered patterns leaderboard
- Weekly bar chart
- Live threat activity feed table

### 🔔 Notification Center

- **Browser push notifications** — works without a server, just the file open in a tab
- **Twilio SMS alerts** — real SMS to your phone on Flag-3 (requires server deploy for CORS)
- **Do Not Disturb schedule** — suppress non-critical alerts during set hours; Flag-3 always bypasses
- **Sound alerts** — Web Audio API, different tones for F2 and F3
- Full notification history log with delivery status

### 👨‍👩‍👧 Parent View

- Live status hero (All Clear / X Critical Alerts)
- 4 live stat cards (Safe Sessions, Alerts Sent, Platforms Watched, Suspects Tracked)
- Recent alerts feed
- Action checklist with specific next steps
- 3 conversation scripts (tested by child psychologists — non-accusatory, effective)
- Warning signs in plain English (no jargon)
- Emergency contacts: NCMEC hotline (tappable), FBI ICAC, CyberTipline, NetSmartz

### 🧪 AI Labs (Claude-powered)

Five exclusive features powered by the Claude API:

1. **Conversation Risk Scorer** — paste a full conversation, Claude returns risk level, grooming stage, red flags, what happens next, and recommended action
1. **Parent Message Drafter** — describe the detection, Claude writes the perfect SMS or email to send to a parent
1. **Suspect Behavior Profiler** — analyze all flagged incidents from a suspect’s profile with full psychological assessment
1. **Grooming Stage Predictor** — identify exactly which of 6 grooming stages a message represents, with visual pipeline and next-step prediction
1. **Multi-Language Detector** — detect grooming in 80+ languages (Spanish, Arabic, Mandarin, Portuguese, etc.) — fills the gap the keyword scanner leaves

### 🔬 AI Analysis Tab

- Batch analysis of all flagged messages via Claude
- Individual message deep-dive
- Pattern confidence scoring
- Export results to JSON

### 📁 Cases

- Full case management (open, pending, closed, escalated)
- Case creation from flagged messages
- Priority levels, notes, platform tracking
- Evidence linking per case

### 🗺️ Geo Map

- Real-time suspect location plotting via ip-api.com (free, 45 req/min)
- Leaflet.js interactive map + SVG world map fallback
- Risk-level color coding (red/amber/blue dots)
- Country/city statistics

### 👤 Suspect Profiles

- Auto-built on every flagged message
- Cross-platform matching — same user on Twitch AND Discord triggers alert
- Risk score formula: `criticalFlags×30 + otherFlags×10 + platformCount×15` (max 100)
- Full incident history per suspect
- Direct report links: tips.fbi.gov, cybertipline.org

### 📝 Evidence Packages

- Court-formatted HTML evidence reports
- Auto-generated case numbers (APEX-XXXXXX)
- Flagged messages with timestamps, platform context, risk scores
- Print/Save as PDF

### 🧠 AI Training

- Label messages as grooming / safe / false positive
- Custom pattern injection (goes live immediately in detection engine)
- Training dataset management (filter, search, export JSON for Hugging Face fine-tuning)
- False positive suppression list

-----

## ⚙️ Settings & Integrations

### Persistent Storage (Supabase)

Connect a free Supabase project to persist cases, profiles, training data, and evidence across sessions.

```sql
-- Run in Supabase SQL Editor
CREATE TABLE IF NOT EXISTS apex_cases (id TEXT PRIMARY KEY, data JSONB, created_at TIMESTAMPTZ DEFAULT NOW());
CREATE TABLE IF NOT EXISTS apex_profiles (id TEXT PRIMARY KEY, data JSONB, updated_at TIMESTAMPTZ DEFAULT NOW());
CREATE TABLE IF NOT EXISTS apex_training (id TEXT PRIMARY KEY, data JSONB, created_at TIMESTAMPTZ DEFAULT NOW());
CREATE TABLE IF NOT EXISTS apex_evidence (id TEXT PRIMARY KEY, data JSONB, created_at TIMESTAMPTZ DEFAULT NOW());
```

### PIN-Protected Parent Mode

Set a 4-6 digit PIN to lock the Parent View. Parents see only their dashboard — no access to investigation tools, evidence, or suspect profiles. Unlocks for 30 minutes per session.

### Webhook Outbound

POST every Flag-3 (or Flag-2, cross-platform) to any HTTPS URL:

- Slack incoming webhooks
- Discord bot webhooks
- Zapier / Make.com
- PagerDuty
- Your own server

Payload format:

```json
{
  "source": "apex-sentry",
  "timestamp": "2026-03-30T12:00:00.000Z",
  "flag_level": 3,
  "username": "SuspectUser",
  "platform": "Twitch",
  "channel": "channelname",
  "message": "the flagged message text"
}
```

### Team Accounts

Invite team members by generating invite codes. Roles: **Analyst**, **Investigator**, **Legal**, **Admin**.

### Voice Alerts

Uses the Web Speech API (built into every browser — no API key). Speaks the alert aloud: platform, username, threat level. Useful when monitoring in the background.

### Weekly PDF Reports

Auto-generates every Sunday, or on-demand. Includes KPIs, top suspect profiles, case summary, platform breakdown. Print → Save as PDF.

### A/B Detection Testing

Run your current detection engine against your labeled training dataset. Returns Accuracy, Precision, Recall, and F1 score. Requires 5+ training examples.

### PWA Install

Install Apex Sentry as a native app on your device:

- **iPhone/iPad**: Share → Add to Home Screen
- **Chrome/Android**: Address bar install icon

-----

## 🌐 Supported Platforms

|Platform               |Connection Method             |Notes                          |
|-----------------------|------------------------------|-------------------------------|
|**Twitch**             |IRC WebSocket (anonymous read)|No API key required            |
|**Kick**               |Pusher WebSocket              |Beta                           |
|**Discord**            |Bot Token + Gateway           |Requires Message Content Intent|
|**YouTube Live**       |Data API v3                   |Requires API key               |
|**Roblox**             |Open Cloud API                |                               |
|**Steam**              |Web API                       |                               |
|**Minecraft**          |RCON                          |                               |
|**Xbox Live**          |Azure AD OAuth2               |                               |
|**TikTok**             |Developer App                 |                               |
|**Facebook Gaming**    |Meta Graph API                |                               |
|**Fortnite/Epic**      |OAuth2                        |                               |
|**PlayStation Network**|NPSSO Token                   |                               |

-----

## 🔑 API Keys Required (optional — features degrade gracefully without them)

|Service                   |Where to get it                                             |Used for                     |
|--------------------------|------------------------------------------------------------|-----------------------------|
|**Claude API** (Anthropic)|[console.anthropic.com](https://console.anthropic.com)      |AI Chat, AI Labs, AI Analysis|
|**Twilio**                |[twilio.com](https://twilio.com)                            |SMS alerts to parents        |
|**Supabase**              |[supabase.com](https://supabase.com)                        |Persistent storage           |
|**YouTube Data API v3**   |[console.cloud.google.com](https://console.cloud.google.com)|YouTube Live monitoring      |

All keys are stored in `sessionStorage` only — never transmitted anywhere except directly to the respective service.

-----

## 🏗️ Architecture

```
apex-sentry-full.html          # Single self-contained file (~903KB)
├── <head>                     # Fonts (Sora + DM Sans), CSS vars, PWA meta
├── <style>                    # ~800 CSS rules, glass morphism, dark/light
├── <nav>                      # Fixed top nav with branding + key indicators  
├── #tabs-bar                  # Draggable tab bar (touch + mouse)
├── .main                      # 19 tab panels
│   ├── #tab-landing           # Hero, stats, mission, quick-connect
│   ├── #tab-monitor           # Multi-channel live feed + right panel
│   ├── #tab-cases             # Case management
│   ├── #tab-sms               # Parent SMS messaging
│   ├── #tab-notifications     # Push + Twilio + DND + log
│   ├── #tab-collab            # Team collaboration
│   ├── #tab-notes             # Investigation notes
│   ├── #tab-discord           # Discord bot installer
│   ├── #tab-admin             # Admin panel
│   ├── #tab-analytics         # Live analytics dashboard
│   ├── #tab-parent            # Parent home dashboard (PIN-protected)
│   ├── #tab-bonus             # AI Labs (5 exclusive Claude features)
│   ├── #tab-platforms         # 12 platform connectors
│   ├── #tab-training          # AI training + custom patterns
│   ├── #tab-aianalysis        # Batch AI analysis
│   ├── #tab-profiles          # Suspect profiles
│   ├── #tab-geomap            # Geographic mapping
│   └── #tab-settings          # All integrations + 10 features
├── AI Chat bubble             # Floating Claude assistant
└── <script>                   # 228 functions, ~9,500 lines
    ├── Detection engine       # RISK_HIGH (69+ patterns) + RISK_MED (53+ patterns)
    ├── Simulation system      # 50 users, 80+ messages, 3 grooming sequences
    ├── Multi-channel system   # Per-channel state, WS management
    ├── Profile builder        # Cross-platform suspect tracking
    ├── Evidence generator     # Court-formatted packages
    └── All 10 settings features
```

-----

## 📊 Detection Engine

### Risk Scoring

```
scoreMsg(text) → { level: 0-3, match: string, color: hex, label: string }
```

### Pattern Categories

- **Linguistic**: Age regression, secrecy induction, isolation tactics, sexual desensitization, meeting facilitation
- **Behavioral**: Rapid escalation, persistence after rejection, platform migration
- **Platform-specific**: Gift bribery (Robux, Nitro, skins), admin impersonation, voice pressure
- **Psychological**: Trauma bonding, love bombing, gaslighting

### Grooming Stages Tracked

1. Targeting (victim selection)
1. Friendship Building
1. Trust Building
1. Desensitization
1. Maintaining Control
1. Exploitation

-----

## 📞 Emergency Resources

|Organization       |Contact                                       |Purpose                         |
|-------------------|----------------------------------------------|--------------------------------|
|**NCMEC**          |1-800-843-5678 (24/7)                         |Missing & exploited children    |
|**FBI ICAC**       |[tips.fbi.gov](https://tips.fbi.gov)          |Internet crimes against children|
|**CyberTipline**   |[cybertipline.org](https://cybertipline.org)  |Report online exploitation      |
|**NetSmartz**      |[netsmartz.org](https://netsmartz.org)        |Free family safety resources    |
|**ICAC Task Force**|[icactaskforce.org](https://icactaskforce.org)|Law enforcement coordination    |

-----

## 📈 Stats

- **903KB** — single HTML file, zero dependencies, zero build step
- **9,532 lines** of code
- **228 JavaScript functions**
- **19 tab panels**
- **12 platforms** monitored
- **122+ detection patterns** (69 high-risk, 53 medium-risk)
- **80+ languages** supported via Claude AI
- **0 servers required** for core functionality

-----

## 📄 License

This project is built for child safety research and protection purposes. Please use responsibly and in accordance with applicable laws regarding monitoring and privacy.

-----

*Built with Anthropic Claude API — Apex Sentry v4.0*
