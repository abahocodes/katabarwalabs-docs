# Access Snapshot & Drift — Setup & Usage

Gives Jira Cloud admins point-in-time access history: a daily snapshot of who can access every project (roles, groups, and effective users), retained long-term, diffed for drift between runs, with audit-ready CSV export and a preview-first drift revert. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.access-snapshot-drift`.

## What it does

Periodic access reviews (SOC 2, ISO 27001, internal audit) need point-in-time evidence: who had access to what on the review date, and what changed since. Jira shows access right now, but its native audit log retains only about 180 days, so once a window rolls off you cannot reconstruct "who could access project X on date Y?". This app fills that gap from inside your own tenant.

- Takes a daily point-in-time snapshot of every project's access: roles, groups, and effective users (group membership expanded).
- Retains snapshots long-term in Forge storage (default about 400 days, far longer than Jira's audit log), then ages them out automatically.
- Diffs each run against the previous one, so you see exactly what access changed: users and groups added or removed per role, and projects newly governed or removed.
- Exports any snapshot as audit-ready CSV, one row per grant.
- Preview-first drift revert rolls back an access addition after a dry-run preview and an explicit confirm.

## Requirements

- A Jira Cloud site and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Read-first for the snapshots and reports; the two manage scopes are used only by the opt-in drift-revert action. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-work` | Read each project's roles and role actors to build the access snapshot. |
| `read:jira-user` | Read user details (accountId, active status, display name) shown in the access matrix. |
| `manage:jira-project` | Used only by the opt-in drift revert, to remove an access addition (a role actor) from a project. |
| `manage:jira-configuration` | Used only by the opt-in drift revert, where reverting a change requires project-configuration scope. |

The app stores its snapshot series in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Access Snapshot & Drift**.

The daily scheduled snapshot populates the series. Select **Snapshot now** to capture on demand.

## How to test it (see it capture and diff)

The drift view is most useful once there are at least two snapshots to compare. To see it work:

1. Open **Jira Settings → Apps → Access Snapshot & Drift** and select **Snapshot now** to capture the first point-in-time snapshot. The access matrix lists each project with its roles and effective users.
2. Change a project's access: as a Jira admin, add a user or group to a project role (Project settings, People or Access).
3. Select **Snapshot now** again. The drift section now shows the delta: the principal you added appears as an access addition for that project and role.
4. Export any snapshot as CSV to get the full access matrix. To try the opt-in cleanup, select a drift addition, review the dry-run preview, and confirm to revert it.

If only one snapshot exists, the drift section is empty until the next run, which is expected.

## Support

- Publisher: Katabarwa Labs (Katabarwa Labs Inc)
- Support contact: Abaho Katabarwa, abaho@llmgraph.ai
- Website: https://katabarwalabs.dev/
- Privacy policy: https://katabarwalabs.dev/privacy
