# Page-Restriction Governance — Setup & Usage

Gives Confluence Cloud admins a site-wide view of every restricted page: who holds read and update on it, which restrictions still name deactivated accounts, what inherits from a restricted ancestor, and what changed since the last scan. Runs entirely on Atlassian Forge inside your own Confluence Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.page-restriction-governance`.

## What it does

Anyone can restrict any Confluence page, and nothing rolls it up. Confluence has no "show me every restricted page and who can see it" view, per space or site-wide, so access reviews become page-by-page archaeology and restrictions quietly rot as people leave. This app computes that view across the whole site, daily and on demand.

- Inventories every restricted page site-wide, grouped by space (personal spaces included).
- Lists the exact users and groups holding read and update on each page.
- Flags orphan restrictions that still name deactivated accounts.
- Notes restricted pages that inherit from a read-restricted ancestor.
- Tracks drift versus the previous run: pages and subjects added or removed.
- Exports the full inventory as audit-ready CSV, one row per page, operation, and subject.
- Optionally removes orphaned restrictions (opt-in, previewed, explicitly confirmed).

## Requirements

- A Confluence Cloud site and a **Confluence administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Read-first. Reporting is read-only; the single write scope is used only by the opt-in orphan-removal action. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:space:confluence` | Enumerate spaces for the site-wide scan. |
| `read:page:confluence` | Enumerate pages to find which are restricted. |
| `read:content-details:confluence` | Read each restricted page's read and update restriction detail. |
| `read:user:confluence` | Check account status of restriction subjects to flag deactivated (orphan) accounts. |
| `write:content.restriction:confluence` | Only used by the opt-in Remove orphaned restrictions action, to delete a deactivated account from the restrictions naming it. Never used by the scan or report. |

The app caches its report in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Confluence site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Confluence Settings → Apps → Page-Restriction Governance**.

The daily scan populates the report. Select **Refresh** to scan on demand.

## How to test it (see it find results)

The report is most useful when there are restricted pages, and it flags orphans only when a restriction names a **deactivated** account. On a site with no restricted pages it correctly reports an empty inventory, which means the site is clean, not that the app did nothing. To see a populated report:

1. Create or pick a page and add a **page restriction** (Restrictions, limit who can view or edit), naming a test user or group.
2. Open **Confluence Settings → Apps → Page-Restriction Governance** and select **Refresh**. The page now appears in the inventory with its exact read and update subjects. Export CSV to get the full list.
3. To see an **orphan** flag: as a Confluence admin, deactivate the test user (Administration, User management), then **Refresh** again. The restriction naming that deactivated account is now flagged as an orphan.
4. To try the opt-in cleanup: select the orphaned restriction, review the **dry-run preview**, and confirm. The deactivated account is removed from the restriction. This action is permanent.

If a scan runs on a site with no restricted pages, the empty result is expected.

## Support

- Publisher: Katabarwa Labs (Katabarwa Labs Inc)
- Support contact: Abaho Katabarwa, abaho@llmgraph.ai
- Website: https://katabarwalabs.dev/
- Privacy policy: https://katabarwalabs.dev/privacy
