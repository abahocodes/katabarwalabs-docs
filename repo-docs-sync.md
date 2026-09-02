# Repo Docs Sync for Confluence — Setup & Usage

Keep Confluence pages true to the code they document. Link a page to a repository path; on demand or on a schedule, your own CI (GitHub Actions or GitLab CI) runs your AI coding agent against the checked-out code and proposes the corrected page. You review the proposal and apply it as a new page version, stamped with the commit it was checked against. We operate no servers.

> Marketplace listing name: **Repo Docs Sync for Confluence**. App key: `dev.katabarwalabs.docs-sync-agent`.

## How it works

1. A page editor links the page to the repository path it documents (for example `src/api/` or `docs/setup.md`).
2. **Sync now** (or the daily/weekly schedule) sends the current page body to your CI.
3. In YOUR CI runner, the agent (Claude Code or Codex) checks out the repository, compares the page against the actual code, and returns a corrected page body.
4. The result lands as a **proposal**: a reviewable update with a change summary and the commit hash. A human applies it in one click; the page version history shows "Repo Docs Sync (against <commit>)". Direct-apply is opt-in per page, and a human edit made mid-sync always wins — the app never overwrites a page that changed.

## Requirements

- A Confluence Cloud site, and a **Confluence administrator** to install the app and store the CI credential.
- A GitHub repository with Actions enabled, or a GitLab project with CI.
- An agent API key stored as a CI secret in your repository (dedicated and spend-capped). It is never given to the app.

## Permissions requested

| Scope | Why |
|-------|-----|
| `read:page:confluence` | Read the linked page body for the sync payload, and check the calling user's page permissions. |
| `write:page:confluence` | Apply an accepted proposal as a new page version. |
| `storage:app` | Store page links, run history, proposals, and credentials (Forge secret storage) in your own site. |

External egress (declared in the manifest): `api.github.com` and `gitlab.com` only. The only data sent is the linked page's content, to the pipeline you configured, when a sync fires.

## Setup (about five minutes)

**1. Store the CI credential (site admin, once)**
Go to **Confluence Settings → Repo Docs Sync** and paste a GitHub fine-grained token (Contents + Workflows read/write on the target repos) or a GitLab pipeline trigger token. Stored in Forge secret storage, never displayed again.

**2. Link a page (page editor, per page)**
Open the page and click **Repo Docs Sync** in the byline under the title.
- Choose provider, repository, base branch, the path the page documents, the agent, apply mode (proposal recommended), and schedule (manual, daily, or weekly).
- If the docs-sync workflow is missing from the repository, the dialog says so — press **Install workflow** to commit it to the repository's default branch in one click. (GitLab: paste the template from the admin page.)

**3. Add your agent key to the repository (once per repo)**
In the repository's CI secrets, add `ANTHROPIC_API_KEY` (Claude Code) or `OPENAI_API_KEY` (Codex).

**4. Sync**
Press **Sync now**. When the proposal appears, review the summary and preview, then **Apply to page** — or **Discard**.

## Governance

- **Page edit permission** is required for every mutation (linking, syncing, applying, discarding) — viewers can only see status.
- **Daily sync cap** site-wide, set on the admin page.
- **Per-run callback tokens**: single-use, dead when the run ends.
- **Version guard**: applying checks the page version the proposal was generated against; if anyone edited the page since, the apply is refused and you re-sync instead.
- **Audit log**: every sync recorded (trigger, outcome, commit), exportable as CSV. Stale runs are swept to failed automatically.

## Troubleshooting

- **"The docs-sync workflow is not installed"**: press Install workflow in the byline dialog (GitHub), or add the `.gitlab-ci.yml` template (GitLab).
- **Proposal seems truncated in the preview**: the preview shows the first 4,000 characters; the full body is applied. Very large pages are sent to CI truncated (flagged in the payload) because of provider payload limits — the agent still reads the full code.
- **"The page has been edited since this proposal was generated"**: someone changed the page after the sync snapshot. Re-sync for a fresh proposal against the current page.
- **Sync failed with agent output attached**: usually a missing or expired API key in the repository's CI secrets.

## Data & privacy

The app stores page links, run records, proposals (chunked), and your CI credential (Forge secret storage) inside your own Atlassian site. What leaves your tenant: the linked page's content, sent to the CI pipeline you configured when a sync fires. This app is not part of the "Runs on Atlassian" program because egress to your CI is the product.

Claude Code is a trademark of Anthropic, PBC. Codex is a trademark of OpenAI. This app is not affiliated with Anthropic, OpenAI, GitHub, or GitLab.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
