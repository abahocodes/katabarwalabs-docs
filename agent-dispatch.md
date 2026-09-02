# Agent Dispatch for Jira — Setup & Usage

Send a Jira work item to your AI coding agent with one click. The agent runs in your own CI (GitHub Actions or GitLab CI) with your own API key, implements the change, and opens the pull request; status and the PR link come back to the issue. We operate no servers: the app's only job is to fire the pipeline you already own and record what happened.

> Marketplace listing name: **Agent Dispatch for Jira**. App key: `dev.katabarwalabs.agent-dispatch`.

## How it works

1. A user presses **Send to agent** on a Jira issue (needs edit permission on the issue).
2. The app packages the work item (summary and description) and fires your CI: GitHub `repository_dispatch` or a GitLab pipeline trigger, using the credential your admin stored.
3. In YOUR CI runner, the agent (Claude Code or Codex) checks out the repository, implements the change, and opens the pull request / merge request.
4. The pipeline reports back over a single-use, per-run callback token. The issue panel shows live status, and a comment with the PR link and the agent's summary lands on the issue.

Your code never leaves your infrastructure, and your agent API key lives only in your CI's secrets.

## Requirements

- A Jira Cloud site, and a **Jira administrator** to install the app and store the CI credential.
- A GitHub repository with Actions enabled, or a GitLab project with CI.
- An agent API key for your CI (for example a dedicated, spend-capped Anthropic or OpenAI key), stored as a CI secret in your repository. It is never given to the app.

## Permissions requested

| Scope | Why |
|-------|-----|
| `read:jira-work` | Read the dispatched issue's summary and description. |
| `write:jira-work` | Post the result comment (PR link and agent summary) on the issue. |
| `storage:app` | Store project configs, run history, and credentials (Forge secret storage) in your own site. |

External egress (declared in the manifest): `api.github.com` and `gitlab.com` only — the two dispatch targets. The only data sent is the dispatched work item's text, to the pipeline you configured.

## Setup (about five minutes)

**1. Store the CI credential (site admin, once)**
Go to **Jira Settings → Apps → Agent Dispatch**.
- GitHub: paste a fine-grained token with **Contents: read/write** and **Workflows: read/write** on the target repositories (add **Administration: read/write** if you want the installer to enable the Actions PR setting for you). Used only to fire `repository_dispatch` and run the one-click installer.
- GitLab: paste a **pipeline trigger token** for the project.
Tokens go into Forge secret storage and are never displayed again.

**2. Wire a project (project admin, per project)**
Go to **Project settings → Agent Dispatch** in the Jira project.
- Choose the provider, the repository (GitHub `owner/name` or GitLab numeric project id), the base branch, and the agent (Claude Code or Codex).
- The **Repository setup** card checks the GitHub side live: is the CI workflow installed, and can Actions create pull requests?
- Press **Install workflow**: the app commits the ready-made workflow to the repository's default branch and enables the PR setting, using the token from step 1. (GitLab: paste the `.gitlab-ci.yml` template from the admin page instead — trigger tokens cannot write files.)

**3. Add your agent key to the repository (once per repo)**
In the repository's CI secrets, add `ANTHROPIC_API_KEY` (Claude Code) or `OPENAI_API_KEY` (Codex). Use a dedicated, spend-capped key.

**4. Dispatch**
Open any issue in the project and press **Send to agent** on the Agent Dispatch panel. Watch the status; the PR link arrives as a comment when the run finishes.

## Governance

- **Issue edit permission** is required to dispatch — viewers and portal customers cannot fire runs.
- **Daily run cap** (site-wide) and a **project allowlist** on the admin page.
- **Per-run callback tokens**: each dispatch mints a single-use token that dies when the run ends; a leaked token is worth one run's status updates, briefly.
- **Audit log**: every dispatch recorded (who, issue, repository, outcome), exportable as CSV from the admin page.
- Stale runs whose CI never responds are automatically marked failed by an hourly sweep.

## Troubleshooting

- **"The CI workflow is not installed"** on the panel: a project admin can fix it in one click from Project settings → Agent Dispatch → Install workflow.
- **GitHub 404 on dispatch**: the repository name is wrong OR the stored token has no access to it — GitHub reports both the same way.
- **Run stuck, then failed with "No response from CI"**: the workflow is missing from the repository's *default* branch (GitHub only triggers `repository_dispatch` from there), or the pipeline crashed before reporting. Check the Actions/pipeline log.
- **Agent step failed**: the failure comment on the issue includes the agent's own last output (commonly a missing or expired API key).
- **PR creation fails with "not permitted"**: enable *Allow GitHub Actions to create and approve pull requests* in the repository's Actions settings, or re-run Install workflow which sets it for you.

## Data & privacy

The app stores project configurations, run records (including the dispatching user's account id), and your CI credential (Forge secret storage) inside your own Atlassian site. What leaves your tenant: the dispatched work item's summary and description, sent to the CI pipeline you configured — nothing else, to nowhere else. This app is not part of the "Runs on Atlassian" program because egress to your CI is the product.

Claude Code is a trademark of Anthropic, PBC. Codex is a trademark of OpenAI. This app is not affiliated with Anthropic, OpenAI, GitHub, or GitLab.

## Support

Questions or issues: **abaho@llmgraph.ai**, or via https://katabarwalabs.dev/support.
