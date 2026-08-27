# Inactive-User Hygiene — Setup & Usage

Gives Jira Cloud admins a view of idle and never-active accounts that still hold a license: it derives each user's last activity from Jira, flags accounts past a threshold you set, and exports a license-reclaim worklist as audit-ready CSV. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.inactive-user-hygiene`.

## What it does

Jira has no "who hasn't touched anything in 90 days?" view, so idle-but-still-licensed accounts quietly consume seats. This app derives activity from Jira and surfaces exactly those accounts, from inside your own tenant.

- Derives each user's last activity from Jira work history (issues created, updated, assigned, and commented).
- Flags idle accounts past a threshold you set, plus never-active accounts, against your licensed users.
- Exports a license-reclaim worklist as audit-ready CSV.
- Optional, admin-confirmed removal of an inactive account from its license-granting groups to reclaim the seat (opt-in and previewed).

## Requirements

- A Jira Cloud site and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Read-first; the manage scope is used only by the opt-in reclaim action. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-user` | Enumerate users and their group memberships to identify licensed accounts. |
| `read:jira-work` | Derive each user's last activity from Jira work history. |
| `manage:jira-configuration` | Used only by the opt-in reclaim action, to remove an inactive account from the groups that grant its license. |

The app caches its derived activity and run state in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Inactive-User Hygiene**.

## How to test it (see it find idle accounts)

1. Open **Jira Settings → Apps → Inactive-User Hygiene**. The app derives last-activity per user and lists idle and never-active accounts against the inactivity threshold. Adjust the threshold (for example, 90 days) to match your policy.
2. Export the reclaim worklist as CSV to get the full list of flagged accounts.
3. Optional cleanup: select an inactive account, review the dry-run preview, and confirm to remove it from its license-granting groups.

Note on test data: on a small or new site most users are recently active, so the idle list may be short. Lower the threshold, or use a site with dormant accounts, to see a populated list. An empty list means no accounts crossed the threshold, not that the app did nothing.

## Support

- Publisher: Katabarwa Labs (Katabarwa Labs Inc)
- Support contact: Abaho Katabarwa, abaho@llmgraph.ai
- Website: https://katabarwalabs.dev/
- Privacy policy: https://katabarwalabs.dev/privacy
