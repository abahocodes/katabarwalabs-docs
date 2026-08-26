# Attachment Cleanup for Confluence — Setup & Usage

Finds where your Confluence attachment storage is going and how much of it is reclaimable waste, then (optionally) cleans it up with a recoverable soft-delete. Runs entirely on Atlassian Forge inside your own Confluence Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.confluence-attachment-cleanup`.

## How the app works

Attachments accumulate silently in Confluence: inline images removed from a page but not from storage, files on pages that were archived or trashed years ago, and large uploads nobody remembers. Confluence has no site-wide view of any of it. This app sweeps the whole site (daily and on demand), inventories every attachment on every page, and classifies each one, then totals the storage you could reclaim.

Each attachment is classified as:
- **Unused** — the file is not referenced in its page's current body.
- **Orphaned** — the file lives on an archived or trashed page (dead weight regardless of references).
- **Large** — over a size threshold (default 10 MiB), worth an eyeball.

## Key features

- Site-wide attachment inventory, swept daily and on demand.
- Per-space storage totals and total **reclaimable bytes** (unused + orphaned), biggest opportunities first.
- Audit-ready CSV export of the full inventory (UTF-8 with BOM, formula-injection guarded, totals stamped).
- Optional, admin-confirmed bulk cleanup that **soft-deletes** flagged attachments to the space Trash (recoverable).

## Requirements

- A Confluence Cloud site and a **Confluence administrator** account to install and open the app.
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

Report-first. The inventory is read-only; the delete scope is reached only through the confirmed cleanup flow. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:space:confluence` | Enumerate spaces (type and status). |
| `read:page:confluence` | Read pages across statuses (current, archived, trashed) to classify orphans. |
| `read:attachment:confluence` | Inventory every attachment and its size and references. |
| `delete:attachment:confluence` | Only used by the opt-in cleanup, to soft-delete an attachment you explicitly confirm. Never used by the inventory path. |

The app stores its report in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Confluence site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-user tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Confluence Settings → Apps → Attachment Cleanup for Confluence**.

A daily scan keeps the inventory current. On first open, select **Refresh** to run the sweep immediately (it runs in the background; the page opens from the cached report once ready).

## Usage

**Review the inventory**
- See per-space storage totals, total reclaimable bytes, and every attachment flagged Unused, Orphaned, or Large, biggest opportunities first.

**Export CSV**
- Select **Export CSV** to download the full inventory for a storage review.

**Clean up (optional, admin-confirmed soft-delete)**
- Tick the flagged attachments you want to remove, select **Preview** to see the exact set (a dry run that changes nothing), then **Confirm**.
- The confirmed delete is a **soft delete**: each attachment moves to its space Trash and is **recoverable** by a space admin. The app never purges and never touches a used, non-flagged file.

## How to test it (verify functionality)

The report only flags attachments that are Unused, Orphaned, or Large. On a clean or empty site it will correctly show little or nothing, which means the site is clean, not that the app did nothing. To see each flag populate, create the conditions it looks for:

1. **Unused**: on any page, insert an image or attach a file, publish, then edit the page and remove that image/file from the body and publish again. The attachment stays in storage but is no longer referenced.
2. **Orphaned**: attach a file to a page, then **archive or trash** that page.
3. **Large**: upload a file larger than the size threshold (default 10 MiB) to any page.
4. Open **Confluence Settings → Apps → Attachment Cleanup for Confluence** and select **Refresh**.
5. The report now lists the attachment under Unused, Orphaned, and Large respectively, with per-space totals and reclaimable bytes. Export CSV to see the full row detail.
6. (Optional) Tick one flagged attachment, **Preview** the cleanup (nothing changes), then **Confirm** to soft-delete it, and confirm it appears in the space Trash and can be restored.

## Notes on scope (honesty first)

The "used" check is a textual reference scan of the page body, not a full render. An attachment surfaced only through an indirect mechanism (for example an attachments or gallery macro) is reported honestly as "not referenced in the current body," and the UI and CSV say exactly that. Treat the report as a review queue, which is why cleanup is always previewed, admin-confirmed, and soft (recoverable). Blog-post and whiteboard attachments are out of scope for this version.

## Data handling

All computation runs on Atlassian's serverless platform inside your Confluence Cloud site. There is no external server and no data egress, so the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
