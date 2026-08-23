# SquidWeave Marketing OS v9.1

Open-source GoHighLevel alternative built for SquidWeave's marketing arm. Now with live email sending and prospecting pipeline integration.

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   v8 Engine     │────▶│   Backend API    │◄────│   v9 Dashboard  │
│  (prospecting)  │     │  (Node.js HTTP)  │     │  (React + Vite) │
└─────────────────┘     └──────────────────┘     └─────────────────┘
                               │
                               ▼
                        ┌──────────────┐
                        │  Brevo SMTP  │
                        │ (free tier)  │
                        └──────────────┘
```

## Quick Start

### 1. Dashboard (Frontend)
```bash
cd app
npm install
npm run dev
# Opens on http://localhost:5173
```

### 2. Backend API
```bash
cd api
node server.mjs
# Runs on http://localhost:3001
```

### 3. Configure SMTP
1. Sign up at [brevo.com](https://brevo.com) (free 300 emails/day)
2. Go to SMTP & API → Create SMTP key
3. Paste key in Settings → SMTP → Username
4. Click "Test Connection"

### 4. Run Prospecting → CRM Pipeline
```bash
cd api
node outreach-pipeline.mjs "crypto startups" --limit 20
# Finds leads via v8 engine and imports into CRM
```

## Features

| Module | Status | Live Functionality |
|---|---|---|
| **Dashboard** | Complete | KPI cards, pipeline chart, activity feed, tasks |
| **CRM** | Complete | Contact management, pipeline board, detail drawer |
| **Campaigns** | Complete | Create campaigns, **send real emails via Brevo SMTP** |
| **Funnels** | Complete | Funnel overview with conversion metrics |
| **Appointments** | Complete | Weekly calendar, booking links, availability |
| **Analytics** | Complete | Email, funnel, traffic, pipeline charts |
| **Prospecting** | Complete | File import, lead finder, **API import to CRM** |
| **Settings** | Complete | SMTP config with test, team, integrations, billing |

## API Endpoints

| Method | Path | Description |
|---|---|---|
| GET | /api/smtp | Get SMTP configuration |
| POST | /api/smtp | Save SMTP configuration |
| POST | /api/smtp/test | Test SMTP connection |
| POST | /api/send-campaign | Send email campaign to contact list |
| GET | /api/campaigns/history | Get campaign send history |
| POST | /api/prospecting/import | Import v8 prospecting results |
| GET | /api/contacts | Get all CRM contacts |

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `PORT` | 3001 | API server port |
| `VITE_API_URL` | http://localhost:3001 | Frontend API URL |

## Live Demo

**Frontend:** https://kqzosuyjbxtro.kimi.page

## Integration with v8 Prospecting Engine

### Import Bridge
```bash
# Manual import
node api/import-bridge.mjs path/to/prospecting-results.json

# Full pipeline (search + import)
node api/outreach-pipeline.mjs "blockchain startups" --limit 10
```

### Data Flow
1. v8 engine queries GitHub API for leads (no paid APIs)
2. Results saved as JSON
3. Import bridge sends to `/api/prospecting/import`
4. Contacts appear in CRM dashboard instantly
5. Create campaign → send via Brevo SMTP

## Tech Stack

**Frontend:**
- React 19 + TypeScript + Vite 7
- Tailwind CSS v3.4.19 + shadcn/ui
- Zustand (state + localStorage persistence)
- Recharts (charts)
- date-fns (date formatting)

**Backend:**
- Node.js built-in HTTP server (zero dependencies)
- Raw SMTP socket implementation (no Nodemailer needed)
- JSON file storage (no database needed)

## License

MIT — built for SquidWeave verifier v9.
