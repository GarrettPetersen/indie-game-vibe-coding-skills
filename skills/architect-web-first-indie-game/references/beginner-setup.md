# Beginner setup: the human checklist

Start by checking existing configuration without exposing credentials. Explain
one short group of steps at a time, with the exact website, folder, or file to
use. Define unfamiliar terms briefly: a repository stores the game and its
change history; a local checkout is its folder on the computer; `.env` is a
local configuration file for credentials used by development tools.

Default to GitHub and Cloudflare unless the user has already completed setup or
explicitly chosen alternatives. Do not repeat account creation or move an
existing project just to match these defaults.

## 1. GitHub account and game repository

Walk the human through creating and verifying a free GitHub account and creating
a repository for the game. Explain public/private visibility and let the user
choose; a public browser game does not require a public source repository.

Use current first-party instructions:

- [GitHub account setup](https://docs.github.com/en/get-started/onboarding/getting-started-with-your-github-account)
- [Creating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

The agent handles installing/configuring Git where appropriate, initializing or
cloning the game checkout, setting the remote, and preparing the first commit.
Use the normal browser/device authentication flow for GitHub access. The human
signs in privately; do not request passwords or authentication tokens in chat.
Preserve existing files and history. Verify the remote and access, and complete
the initial commit/push when included in the requested setup workflow.

For a chosen alternative, establish the equivalent local history, remote or
backup, and access verification rather than requiring GitHub.

## 2. Free Cloudflare account and local credentials

Walk the human through signing up for the free Cloudflare account, verifying
the account, finding the account ID, and creating an API token scoped to the
account and services the agent needs to set up. Hosting, telemetry ingestion,
and storage may need different permissions. Determine the required permissions
from current documentation for the planned operations rather than requesting
an unrestricted token or the Global API Key.

- [Create an API token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
- [Cloudflare Pages setup](https://developers.cloudflare.com/pages/get-started/git-integration/)
- [Current Pages Free-plan limits](https://developers.cloudflare.com/pages/platform/limits/)

A custom domain or paid plan is not a prerequisite for the first prototype.
Honor another provider when explicitly selected and use its equivalent scoped
credentials.

Before the human enters any values, the agent prepares:

- a project-local `.env` containing empty required variable slots;
- ignore rules covering `.env` and other actual secret files;
- an optional committed `.env.example` containing variable names and empty
  values only; and
- deployment tooling that loads the local file without putting credentials in
  browser code or public assets.

For Cloudflare tooling, the usual variable names are `CLOUDFLARE_API_TOKEN` and
`CLOUDFLARE_ACCOUNT_ID`; verify what the chosen tool expects. Preserve unrelated
existing `.env` entries. Tell the human the exact local path and how to open it
in their editor, then have them paste the values there and save. Ask only for
confirmation that entry is complete, never for the values themselves.

## Verify privately and continue

Confirm that required values are present, that the secret file is ignored and
untracked, and that a minimal authenticated operation succeeds. Output only
presence/status and sanitized error context. Do not print or search the secret
file, echo environment variables, enable shell tracing, place tokens in command
arguments, or include raw request headers in logs. A `.env` file is not
automatically loaded by every tool; configure loading explicitly.

If the token lacks a necessary permission, explain the exact missing permission
and guide the human to correct it privately. Once access works, the agent
configures the requested hosting, endpoint, storage, and deployment workflow
and verifies the resulting build. Record non-sensitive setup status in project
documentation so future work does not make the user repeat these steps.
