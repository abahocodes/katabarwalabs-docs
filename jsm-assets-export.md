# JSM Assets Export — Setup & Usage

A bulk export and backup for Jira Service Management Assets (CMDB): pick an object schema and download every object and its attributes as CSV or JSON. Runs entirely on Atlassian Forge inside your own Jira Cloud site. Nothing leaves your instance.

> App key: `dev.katabarwalabs.jsm-assets-export`.

## How the app works

Assets lets you browse and AQL-search objects, but there is no one-click "back up this whole object schema" to a portable file. This app fills that gap: you pick an Assets object schema and a format, and the app pages every object in the schema (via AQL), flattens each object's attributes to display values, and streams the finished file straight to your browser. Each completed export is recorded in a rolling history.

## Key features

- Export an entire Assets object schema in one action.
- **CSV** (one row per object, audit-ready) or **JSON** (full-fidelity, re-import-friendly).
- Handles large schemas: the export runs as a background job and streams down in chunks.
- Export history: schema, format, object count, object-type breakdown, timestamp, and a truncation flag.
- Optional, heavily-gated stale-object cleanup (see below).

## Requirements

- A Jira Cloud site with **Jira Service Management and Assets**, and a **Jira administrator** account to install and open the app.
- At least one Assets object schema with objects (to export).
- No external service and no configuration file. The app runs on Atlassian Forge.

## Permissions requested

The export path is read-only. At install you will be asked to approve:

| Scope | Why |
|-------|-----|
| `read:servicedesk-request` | Resolve the Assets workspace id. |
| `read:cmdb-schema:jira` | List Assets object schemas. |
| `read:cmdb-type:jira` | List a schema's object types. |
| `read:cmdb-object:jira` | Page a schema's objects and attributes for the export. |

The app stores export history and the temporary export file in Forge storage inside your site (`storage:app`).

## Install

1. From the Atlassian Marketplace listing, select **Try it free** (or **Buy it now**), and choose your Jira site.
2. Approve the scopes shown above.
3. Wait for the install to finish.

Free for up to 10 users, then standard Atlassian per-agent tiered pricing. Atlassian bills your site directly.

## Open the app

Go to **Jira Settings → Apps → JSM Assets Export**.

## Usage

1. Open the app. It lists your Assets **object schemas**.
2. Pick a schema and choose a format: **CSV** or **JSON**.
3. Start the export. It runs in the background; the page polls for status and then downloads the finished file.
4. The **export history** records the schema, format, object count, object-type breakdown, and timestamp for each run.

## How to test it (verify functionality)

1. Make sure your site has JSM Assets with at least one object schema that contains a few objects. If needed, create a small schema (for example "Test Schema") with an object type and two or three objects.
2. Open **Jira Settings → Apps → JSM Assets Export**.
3. Select that schema, choose **CSV**, and run the export. A CSV downloads with one row per object and columns for each attribute.
4. Run it again with **JSON** and confirm you get a full-fidelity JSON file of the same objects.
5. Open the **export history** and confirm the run is recorded with the correct schema name, format, and object count.
6. Open the downloaded file and confirm the objects and attribute values match what you see in Assets for that schema.

## Stale object cleanup (optional, irreversible)

Separate from export, the app offers an optional cleanup of stale objects. Because **Assets has no trash or recycle bin, a deleted object cannot be restored**, so this action is heavily gated:

- It is **preview-first** (a dry run that lists exactly what would be deleted and changes nothing).
- It requires explicit confirmation: an irreversibility acknowledgement checkbox and typing the schema key.
- It re-checks each object immediately before deletion.

Because it is irreversible, use it only after you have taken an export/backup with this app first. If you only want to back up your Assets data, ignore this feature entirely; the export path never deletes anything.

## Data handling

All computation runs on Atlassian's serverless platform inside your Jira Cloud site. The export file is held only temporarily in Forge storage inside your site and streamed to your browser. There is no external server and no data egress, so the app is eligible for the "Runs on Atlassian" trust badge. See the privacy statement at https://katabarwalabs.dev/privacy.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
