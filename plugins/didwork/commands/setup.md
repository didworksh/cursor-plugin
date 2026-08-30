---
name: setup
description: Guided DidWork setup — prove the connection, detect this project's stack, and backfill verdicts on recent real work
---

# /setup

Get this project from "DidWork is installed" to a populated verification log. Do the steps in order, skip any that don't apply, and keep the user informed as you go. Never fabricate a verdict — every verdict you report must come from a `did_verify` result.

## 1. Prove the connection

Call `did_verify` with `{"type":"http.ok","expected":{"url":"https://didwork.sh"}}`.

- Tool missing → the MCP server isn't loaded; tell the user to restart Cursor and re-run `/setup`.
- Result mentions keyless mode or an `api_key_required` error appears in later steps → the server is running without a key. Point the user at https://didwork.sh/console — creating a key shows an **Add to Cursor** button and install snippets with the key filled in. Continue with what works keyless (`http.ok`) and say what a key unlocks.
- Verdict lands → say so: the pipe works.

## 2. Detect the stack

Look at the repo for systems DidWork can verify — dependency manifests (package.json, requirements.txt, Gemfile, go.mod), `.env.example` variable names, `.github/workflows/`, and config files. Map what you find:

| Found | Claim types it unlocks | Provider to connect |
| --- | --- | --- |
| Stripe SDK / `STRIPE_` vars | `stripe.refund`, `stripe.payment_succeeded`, `stripe.subscription_*`, `stripe.invoice_paid` | Stripe (restricted read-only key) |
| GitHub remote / workflows | `github.pr_merged`, `github.workflow_passed`, `github.release_published`, `github.commit_in_branch` | GitHub (public repos work unconnected) |
| GitLab remote | `gitlab.mr_merged`, `gitlab.pipeline_passed` | GitLab |
| Sentry config | `sentry.issue_resolved` | Sentry |
| Linear / Jira references | `linear.issue_completed`, `jira.issue_done` | Linear / Jira |
| Resend / email sending | `email.delivered` | Resend |
| Slack SDK / webhooks | `slack.message_posted` | Slack |
| A deployed URL (README, config) | `http.ok` | none |

Report what you detected, then recommend the one to three providers actually worth connecting, with the link: https://didwork.sh/console/providers (read-only credentials). Don't wait for the user to connect them — continue with what runs now.

## 3. Backfill: verify recent real work

Populate the log with verdicts about this project's own history. Gather identifiers from `git log`, the `gh` CLI if available, or the GitHub API for public repos — ask rather than guess when an identifier is ambiguous.

- The last ~5 merged PRs → `github.pr_merged` with `{"repository":"owner/repo","pull_request":N}`.
- The most recent CI runs → `github.workflow_passed`.
- The latest release, if any → `github.release_published`.
- The production/deployed URL, if one is known → `http.ok`.

Run each through `did_verify` and present a compact table: claim, subject, verdict. A `failed` or `unknown` here is signal, not a setup problem — explain what the evidence says.

## 4. Offer one watch

If a production URL or an outcome worth monitoring surfaced, offer to create a `did_watch` for it (free keys: one watch, `1h` or slower). Ask before creating it — it's a standing background job. If it's refused with a 402, say so; don't retry.

## 5. Wrap up

Point the user at their log — https://didwork.sh/console/verifications — and restate the working rule: after any consequential action, verify with `did_verify` before reporting success; gate on `verified`, stop on `failed`, never treat `unknown` as done.
