# Project Export & Backup — Setup & Usage

Gives Jira Cloud admins a per-project export: back up a whole project (its issues with fields, comments, changelog, links, and worklogs) to audit-ready CSV or JSON, then optionally archive or trash the project with the backup as the audit trail. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.project-export-backup`.

## What it does

Jira Cloud can export a filtered issue search to CSV, but there is no one-click "back up this whole project" with fields, comments, changelog, links, and worklogs together. Per-project export is one of the longest-standing asks on the Jira Cloud issue tracker (JRACLOUD-34307). This app fills that gap from inside your own tenant.

- Pick any project you can read, and choose CSV (one row per issue, audit-ready) or JSON (structured and re-import-friendly).
- Captures core fields, per-issue comments and worklogs, changelog, issue links, and attachments as URLs plus metadata, with a manifest (project, issue count, generated timestamp, exported fields).
- Streams the finished file straight to your browser.
- Optionally, and heavily guarded, archives or trashes the project after export, using the backup as your audit trail.

## Requirements

- A Jira Cloud site and a **Jira administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Read-first for the export; the two manage scopes are used only by the opt-in archive or trash decommission action. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:jira-work` | Read the project's issues, fields, comments, changelog, links, and worklogs for the export. |
| `read:jira-user` | Resolve user details (assignee, reporter, comment and worklog authors) shown in the export. |
| `manage:jira-project` | Used only by the opt-in decommission action, to archive or trash the project after export. |
| `manage:jira-configuration` | Used only by the opt-in decommission action, where the operation requires project-configuration scope. |

The app stores export history and run state in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → Project Export & Backup**.

## How to test it (see it export)

1. Open **Jira Settings → Apps → Project Export & Backup**.
2. Select a project you can read, choose **CSV** or **JSON**, and start the export. The app pages the project's issues, gathers comments, changelog, links, and worklogs, and downloads the file to your browser.
3. Open the downloaded file: for CSV, one row per issue with the configured fields; for JSON, the structured project export plus a manifest.
4. The archive or trash decommission action is optional and heavily guarded (explicit confirmation), and is not required to export. Only use it when you intend to decommission the project, with the export as your backup. This action is permanent.

## Support

- Publisher: Katabarwa Labs (Katabarwa Labs Inc)
- Support contact: Abaho Katabarwa, abaho@llmgraph.ai
- Website: https://katabarwalabs.dev/
- Privacy policy: https://katabarwalabs.dev/privacy
