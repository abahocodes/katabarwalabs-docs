# Portal Governance for JSM — Setup & Usage

Audits who can reach every Jira Service Management portal, and (optionally) cleans up stale access, running entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> Marketplace listing name: **Portal Governance for JSM**. App key: `dev.katabarwalabs.jsm-portal-governance`.

## What it does

JSM gives admins no single view that answers "who can reach this portal, and how?" Access sprawls: customers are added desk by desk, organizations drift, agents land on the Service Desk Team role directly or through groups, and deactivated accounts linger. This app builds that view and keeps it current.

- Scans every service desk daily (and on demand) and builds a per-desk **access matrix**: customers, organizations with membership rollups, and agents with how each was granted.
- Raises portal-exposure flags (for example a directly-granted agent whose account is deactivated).
- **Drift**: from the second run on, diffs each scan against the previous snapshot (customers/orgs/agents added or removed).
- Exports the whole matrix as audit-ready CSV.
- Optional **Portal access cleanup** turns findings into action (see "Cleanup" below).

## Requirements

- A Jira Cloud site with Jira Service Management, and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Read-only by default, with one opt-in write feature. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:servicedesk-request` | Read service desk, customer, and organization access. |
| `read:jira-work` | Read project roles to resolve Service Desk Team agents. |
| `read:jira-user` | Resolve account details (including deactivated status) shown in the matrix and CSV. |
| `manage:servicedesk-customer` | Only used by the opt-in Portal access cleanup, to remove stale portal access. Never used by the scan/report path. |

The app stores its report in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-agent tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Portal Governance for JSM**.

On first open the report may show as "building" while the first scan runs. After that the admin page opens instantly from the cached report.

## Usage

**Review the access matrix**
- The admin page shows, per service desk, the customers, organizations (with member rollups), and agents (with grant path), plus any exposure flags.
- Select **Refresh** to rescan on demand; the daily scan keeps it current otherwise.

**Check drift**
- From the second scan on, the page shows what changed since the last snapshot: customers, organizations, and agents added or removed per desk.

**Export CSV**
- Select **Export CSV** to download the full matrix (and drift markers) for access reviews and audit evidence.

## Cleanup (optional, opt-in write)

The scan and report path changes nothing. The only writes the app can issue are the **Portal access cleanup** actions, and they are strictly gated:

- **Preview first**: cleanup is always previewed as a dry run before anything is changed.
- **Admin-confirmed**: you explicitly confirm before any action runs.
- **Live re-verified**: every item is re-checked live immediately before removal, so anything that no longer qualifies is skipped with a reason.
- **What it does**: removes stale (deactivated) customers from a desk, removes deactivated users from a customer organization, and removes empty organization grants.
- **What it never does**: it does not deactivate any Atlassian account and does not free any license or seat. It removes portal access only.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. There is no external server and no data egress, so the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
