# Orphaned-Owner Cleanup — Setup & Usage

Finds everything deactivated Jira users left behind (saved filters, dashboards, assigned/reported issues, project-lead roles) and gives admins the work-queue to reclaim it. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.orphaned-owner-cleanup`.

## What it does

When someone leaves and their account is deactivated, their filters, dashboards, issues, and project-lead roles keep pointing at a dead account, and Jira gives admins no way to even find this debris. This app scans the whole site daily (and on demand), classifies every artifact still owned by or pointing at a deactivated account, groups it by type and by departed owner, and exports the lot as audit-ready CSV.

### What it scans

| Orphan type | Meaning |
|-------------|---------|
| `filter` | Saved filters owned by a deactivated user. |
| `dashboard` | Dashboards owned by a deactivated user. |
| `issue-assignee` / `issue-reporter` | Issues still assigned to or reported by a deactivated user. |
| `project-lead` | Projects whose lead is deactivated. |

## Requirements

- A Jira Cloud site and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Report-first, write only on explicit confirmation. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-user` | Enumerate users and detect which accounts are deactivated. |
| `read:jira-work` | Scan filters, dashboards, issues, and project leads to find orphaned artifacts. |
| `write:jira-work` | Only used by the opt-in bulk-reassign action, to reassign issue assignees and filter owners to an active user you choose. Never used by the scan/report path. |

The app caches its report in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Orphaned-Owner Cleanup**.

The daily scan populates the report. Select **Refresh** to scan on demand.

## How to test it (see it find results)

The app only lists artifacts tied to **deactivated** users. On a clean site with no deactivated users, it correctly reports "No orphans, nothing points at deactivated users", which means the site is clean, not that the app did nothing. To see a populated report, create the condition it looks for:

1. Pick a test user (or invite one). As that user, or as admin on their behalf, create something owned by them: save a **filter**, create a **dashboard**, assign an **issue** to them, or set them as a **project lead**.
2. As a Jira admin, go to **Administration → User management** and **deactivate** that user.
3. Open **Jira Settings → Apps → Orphaned-Owner Cleanup** and select **Refresh**.
4. The report now lists the orphaned artifacts, grouped by type and by departed owner. Export CSV to get the full work-queue.

If a scan runs before any user is deactivated, the empty result is expected.

## Usage

**Review the report**
- The admin page groups orphaned artifacts by type and by departed owner, so you can see, per person who left, exactly what they still own.

**Export CSV**
- Select **Export CSV** for the full list to hand off for reassignment and reclamation.

**Bulk reassign (optional, opt-in write)**
- Pick a successor (an active user), **preview** the change (DRY_RUN is the default, so preview changes nothing), then confirm to apply.
- Reassign covers **issue assignees** and **saved-filter owners** only.
- **Dashboards and issue reporters are reported but never auto-changed**; reassign those manually.

## Notes

Some filters are visible to Jira only with elevated share permissions. If the site restricts that, the scan automatically retries in a reduced mode and shows a banner that private filters may be missing while everything else stays fully covered.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. There is no external server and no data egress, so the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
