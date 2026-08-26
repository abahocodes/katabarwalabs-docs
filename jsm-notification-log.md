# Notification Log for JSM — Setup & Usage

A durable, searchable, exportable record of every outgoing Jira Service Management customer notification, running entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> Marketplace listing name: **Notification Log for JSM**. App key: `dev.katabarwalabs.jsm-notification-log`.

## What it does

Jira Service Management has no screen that lists the notifications a service desk has sent, and Atlassian's built-in email log only records the ones that fail. So "did the customer actually get our reply?" has no searchable answer. This app records each outgoing notification as it fires and keeps it in your own site, searchable and exportable.

- Records request created, public reply, internal note, and status change events as they happen, with the request, service desk, actor, and recipient set.
- Fully read-only. It records and exports; it changes nothing in JSM or Jira.
- Records from install forward. History before install is not backfilled.
- Retention window you control (default about 13 months); older entries are pruned automatically.
- Search and filter the log from an admin page.
- One-click, audit-ready CSV export.

## Requirements

- A Jira Cloud site with Jira Service Management, and a **Jira administrator** account to install and open the app.
- No external service, no account with Katabarwa Labs, and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

The app is read-only and least-privilege. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-work` | Read issue and comment context so each notification event can be classified and summarized. |
| `read:servicedesk-request` | Confirm the request is a JSM service desk request and resolve the public/internal audience. |

It stores its log in Forge storage inside your site (`storage:app`). It requests no write scopes.

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the read-only scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-agent tiered pricing. Atlassian bills your site directly. There is no vendor invoicing.

## Open the app

Go to **Jira Settings → Apps → Notification Log for JSM**.

The log starts recording from the moment the app is installed. Send or reply to a JSM request and the entry appears here.

## Usage

**Browse and search**
- The admin page lists recorded notifications newest first, paged on the server.
- Filter by criteria (for example date range, service desk, or audience) to narrow to what an auditor or teammate asked about.

**Digest**
- A summary digest (totals and breakdowns) is recomputed nightly, and you can recompute it on demand from the admin page.

**Set retention**
- Adjust the retention window from the admin page. Entries older than the window are pruned automatically on the daily rollup. Default is about 13 months.

**Export CSV**
- Select **Export CSV** to download the log as a flat file: one row per notification with timestamp, request, service desk, actor, audience, and recipient summary.
- Hand the file to compliance for delivery evidence, access reviews, or an incident investigation.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. There is no external server and no data egress, which is why the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
