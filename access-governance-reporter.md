# Access Governance Reporter — Setup & Usage

Answers the question Jira cannot: who can access what, and which group or role grants it. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.access-governance-reporter`.

## What it does

Jira shows you a permission scheme, or a group's members, but never the reverse lookup: for a given user, every project and permission they effectively hold and the exact group or role path that confers it. This app computes that effective-access matrix across your whole site, plus a group-usage view, and exports both as audit-ready CSV for access reviews and attestations.

- **Effective-access matrix**: per user, every project and permission, and how it was granted (group, role, or direct). Over-broad and direct grants are flagged.
- **Group usage**: for each group, every project it grants access to and how (permission-scheme grant vs project-role actor), with over-broad and unused groups flagged.
- **One-click CSV export** for SOC 2 / ISO access reviews and periodic attestations.

## Requirements

- A Jira Cloud site and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Report-first. The scan and export are read-only; the two management scopes are used only by the optional, admin-confirmed revocation action. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-work` | Read projects, permission schemes, and roles to compute effective access. |
| `read:jira-user` | Resolve user/account details for the matrix and CSV. |
| `manage:jira-configuration`, `manage:jira-project` | Only used by the optional revocation action, to remove an over-broad or stale grant that you explicitly confirm. Never used by the scan/report path. |

The app caches its report in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Access Governance Reporter**.

A daily scan keeps the matrix current. On first open, select **Refresh** to run the scan immediately (it runs in the background; the page opens instantly from the cached report once ready).

## Usage

**Review the effective-access matrix**
- See, per user, every project and permission they hold and the group or role that grants it. Over-broad or direct grants are flagged for review.

**Review group usage**
- See where each group is actually used: members, projects granted, grant path, and whether the group is over-broad or unused. This is the view you need to clean up group sprawl safely.

**Export CSV**
- Select **Export CSV** to download the full matrix, one row per user, project, permission, and grant path, for compliance reviews and attestations.

## Optional revocation (admin-confirmed write)

The scan and export change nothing. An optional revocation action can remove an over-broad or stale grant. It is previewed first, requires explicit admin confirmation, and is re-verified live immediately before any change, so anything that no longer qualifies is skipped with a reason. It removes access only; it does not deactivate accounts.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. There is no external server and no data egress, so the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
