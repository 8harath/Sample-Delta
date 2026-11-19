# Engagement Office — local MVP prototype

A static, browser-only partner portfolio tracker. Open `index.html` directly in a current desktop browser. There is no server, package install, Python, Java, build step, or network requirement for the application logic. Google Fonts are optional; system fallbacks are included.

## Included

- Portfolio dashboard with record-based engagement health, progress, budget, invoice exposure, next-30-day milestones, and a cost-versus-delivery variance view.
- Ten fictional partner portfolios with 15 sample engagements each (150 records total); partner summaries and the complete engagement register are visible from the dashboard.
- Exception queue flags at-risk/on-hold work and engagements where budget burn is materially ahead of recorded delivery progress.
- Engagement register with search by engagement, client, partner, or manager; status filter; edit and delete actions.
- Engagement detail view with team, dates, commercial identifiers, SOW/EMSA states, documents and SharePoint folder link.
- Deadline and people views, plus a setup page describing storage and automation choices.
- Create, edit, delete, JSON import, and JSON export.
- Direct local JSON file connection in compatible browsers, with automatic writes after each record change.
- Architecture diagram: [`architecture.svg`](architecture.svg).
- Fictional starter records: [`data.json`](data.json).

## Local storage behavior

Browsers intentionally restrict a `file://` page from silently reading and rewriting neighboring files. The demo starts with fictional seed records in `app.js`. For a JSON file to be the live local source of truth, click **Connect JSON** and select `data.json` (or use **Data & setup → Create new JSON file**). In compatible current Chromium browsers, the app reads that selected file and automatically writes create, edit, and delete changes back to it after granting file access. Reconnect the file when you open a new browser session. The browser keeps a localStorage fallback; Download and Import remain available for browsers without direct file access. Avoid editing the same file from multiple app windows at the same time.

All 10 partner names and 150 engagement records in the demo, including client names, SharePoint URLs, IDs, amounts, margins, and dates, are fictional examples. Do not put real confidential engagement data in this local demo. Replace example IDs and links using approved sources and naming rules.

## Why email alerts do not run here

A static file has no trusted scheduled process, shared data source, identity, or mail credentials. Browser JavaScript is not a safe place to store Microsoft Graph secrets and cannot reliably run when the laptop/browser is closed. The dashboard displays approaching milestone and invoice dates, but does not send email.

For a Microsoft-first pilot, configure:

1. **Recipient rules:** owner/manager/team recipient mapping, deadline types, lead time (for example 14, 7, and 1 days), escalation, timezone, and duplicate-alert rules.
2. **Approved mail identity:** Microsoft Entra ID tenant and app registration, admin consent, least-privilege Microsoft Graph mail permissions (or an approved organizational mail relay), sender mailbox, and secret/certificate management. Confirm security review and internal policy before selecting app-only or delegated permissions.
3. **Scheduled execution:** Power Automate recurrence flow for a low-code pilot, or an Azure Function timer trigger for more control. Read due records, calculate reminder windows, send mail, log results, and record sent-alert keys to prevent duplicates.
4. **Shared records and access:** replace browser storage with a governed list/database and enforce partner/manager access. Validate retention, audit, backup, and data residency requirements.

No real credentials or tenant configuration belong in this prototype.

## Suggested next stage

- **Fast Microsoft ecosystem pilot:** SharePoint Lists as the system of record, Microsoft Entra ID for access, Power Automate for scheduled reminders, and Power BI for portfolio reporting. This aligns well with the EYOS/SharePoint workflow described, subject to internal IT/security approval.
- **More structured product:** a relational database such as Azure SQL or managed PostgreSQL for engagements, people, assignments, financial periods, milestones, invoices, alert history, and audit records; a small authenticated API/service; and a static frontend hosted in an approved internal environment. Use Microsoft Entra ID SSO and Graph or an approved mail service.

Choose based on record volume, concurrent editors, sensitivity, audit requirements, and who will own operations. A JSON file is suitable for this local demonstration, not a shared source of truth.

## Architecture

```mermaid
flowchart LR
  P[Partner / Manager] --> UI[Static dashboard<br/>HTML · CSS · JavaScript]
  UI --> LS[Browser localStorage<br/>fallback copy]
  JSON[Connected local JSON file<br/>automatic read / write]
  JSON <--> UI
  UI -. future authenticated requests .-> API[Application API]
  API --> DB[(SharePoint Lists<br/>pilot)\n-or-\nAzure SQL / PostgreSQL]
  ID[Microsoft Entra ID] --> API
  SCHED[Power Automate<br/>or Azure Functions timer] --> DB
  SCHED --> GRAPH[Microsoft Graph / approved mail relay]
  GRAPH --> MAIL[Partner / manager email]
  DB --> BI[Power BI reporting]
```

## JSON field guide

The Partners directory summarizes open work, progress, budget remaining, and review items for each partner. Select any partner to open that partner’s full engagement register. The dashboard and register include partner filters; the full dashboard register can be searched by partner, client, engagement, or manager.

The schema uses illustrative identifiers: `OP-######`, `PACE-########`, `EIF-YY-####`, `TY25-XXX-####`, `CYSW-YY-####`, and `CYMSA-YY-####`. These are examples only, not verified organizational formats. Confirm authoritative formats, required fields, allowed status values, and which year columns are still required with the engagement operations/finance team before production use.

`engagements[]` holds one engagement per record. Common fields include `id`, `client`, `name`, `partner`, `manager`, `status`, `progress`, `startDate`, `endDate`, `milestoneDate`, `budget`, `spent`, `invoiceDue`, `invoiceDate`, `team[]`, `sharepoint`, `documents`, annual OP/PACE/pricing/margin/EIF/code/EYOS fields, `sow`, `emsa`, `cysw`, `cymsa`, and `notes`.

## Notes

The dashboard uses aggregate fields from each engagement record. Cost-versus-delivery variance is a directional management signal (`spent ÷ budget` compared with recorded progress), not a forecast or accounting calculation. It only shows the five open engagements with the highest budget burn.

This directory is intended for local review. GitHub publishing can be done once you approve the local experience and share the target repository/link. No external service has been connected, and nothing has been pushed.
