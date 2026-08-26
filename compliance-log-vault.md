# Compliance Log Vault — Setup & Usage

Long-term Jira audit-log retention and export, running entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

## What it does

Jira's own audit log is capped, so records older than roughly 180 days age out and are gone. Compliance Log Vault runs a daily background sync that copies new audit records into a vault held in your own site (Forge storage), well past that window, and lets a compliance admin browse the history and export it as audit-ready CSV for SOC 2 / ISO evidence.

- Daily incremental sync of new Jira audit records into an in-tenant vault.
- Dedup by record id, so a record is never stored twice, even across overlapping syncs.
- Long-term retention past Jira's ~180-day cap. The vault only grows.
- Browse retained records (newest first) from an admin page.
- Retention stats: counts by day and by category, and the full retained span.
- One-click CSV export of the whole vault.

## Requirements

- A Jira Cloud site, and a **Jira administrator** account to install and open the app.
- No external service, no account with Katabarwa Labs, and no configuration file. The app runs on Atlassian Forge with read-only scopes.

## Permissions requested

The app is read-only and least-privilege. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:audit-log:jira` | Read Jira audit records (`GET /rest/api/3/auditing/record`). |
| `read:jira-user` | Resolve the author/account details shown in the browser and CSV. |

It requests no write scopes and changes nothing in Jira. It only reads audit records and appends them into its own Forge storage vault.

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the read-only scopes shown above.
3. Wait for the install to finish. The app is now available under your Jira apps.

Free for up to 10 users, then roughly $1.20 per user per month with standard Atlassian volume tiers. Atlassian bills your site directly. There is no vendor invoicing.

## Open the app

Go to **Jira Settings → Apps → Compliance Log Vault**.

The first time you open it, the vault may be empty. It fills automatically on the daily sync, or you can populate it immediately (next step).

## Usage

**Populate the vault**
- The app runs a **daily scheduled sync** on its own. No action is needed to keep it current.
- To pull records right away, select **Sync now** on the admin page. This fetches every audit record available since the last stored point and merges it in, deduped by id.

**Browse retained records**
- The admin page lists retained records newest first, paged on the server so large vaults stay responsive.
- Use the date filters to narrow to a specific window (for example, the period an auditor asked about).

**Check retention stats**
- The page shows counts by day and by category, and the full span currently retained, so you always know how far back your evidence goes and roughly how much capacity is used.

**Export CSV**
- Select **Export CSV** to download the full retained vault as a flat file: one row per audit record with id, timestamp, author, summary, category, object item, and changed values.
- Hand the file to compliance for access reviews, SOC 2 / ISO evidence, or an incident investigation.

## Retention and limits

Retention is bounded by Forge app storage quotas, which is typically years of history at moderate audit volumes. Records are sharded per month so no single stored value hits the storage cap. The admin page shows the approximate capacity used, so the bound is never a surprise.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. There is no external server and no data egress, which is why the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
