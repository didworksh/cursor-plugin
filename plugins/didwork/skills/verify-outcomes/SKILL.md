---
name: verify-outcomes
description: Independently verify that an intended outcome actually happened using DidWork. Use after any action with an external side effect — a refund issued, a PR merged, a pipeline passed, an email delivered, a deploy live — before reporting success or acting on it.
---

# Verify outcomes with DidWork

A function can execute, an API can return `200`, and an agent can report "done" without the intended outcome being true. DidWork receives a **claim**, gathers **evidence** from the authoritative system, and returns a **verdict**: `verified`, `failed`, or `unknown`.

## When to use

- You (or code you ran) performed an action in an external system and are about to report it as done.
- A next step depends on a previous outcome being real (e.g. notify the customer only after the refund actually exists).
- You need to confirm a deploy, workflow, or endpoint is healthy.

## Instructions

1. Pick the most specific claim type for the outcome. Common types:
   - `stripe.refund`, `stripe.payment_succeeded`, `stripe.subscription_cancelled`, `stripe.subscription_active`, `stripe.invoice_paid`
   - `github.pr_merged`, `github.workflow_passed`, `github.issue_closed`, `github.release_published`, `github.commit_in_branch`, `github.file_exists`
   - `gitlab.mr_merged`, `gitlab.pipeline_passed`, `gitlab.issue_closed`
   - `linear.issue_completed`, `jira.issue_done`, `sentry.issue_resolved`
   - `email.delivered`, `http.ok` (any public URL — works without an API key)
2. Call `did_verify` with `type` and `expected` fields, e.g. `{ "type": "stripe.refund", "expected": { "payment": "pi_123", "amount": 4999 } }`.
3. Gate on the verdict:
   - `verified` — proceed and cite the evidence.
   - `failed` — stop, report the failure reason from the evidence, and fix before retrying.
   - `unknown` — treat as "not yet"; poll with `did_get` or re-verify. Never treat unknown as success.
4. For outcomes that must remain true over time, create a watch with `did_watch` (interval like `10m` or `1h`, optional `webhook_url`). List with `did_watches`, stop with `did_unwatch`.
5. Review recent verifications and evidence with `did_list`; check quota with `did_usage`.
6. Before calling a project's own agent tool with real-world consequences, call `did_inspect_tool` (optionally with `tool` for the full profile). It returns the tool's evidenced capabilities with `file:line` evidence, a risk level, a separate confidence level, and `DECLARATION_MISMATCH` when the implementation exceeds what the tool declares. Treat `unknown` as "not shown to be safe". When verifying work such a tool performed, pass its profile in `did_verify`'s `tools` so the receipt records what the tool could affect — it is context on the receipt, not evidence for the verdict.

## Setup

Requires `DIDWORK_API_KEY` in the environment (get one at https://didwork.sh/console). `http.ok` claims work with no key. The full claim-type reference lives at https://didwork.sh/docs.
