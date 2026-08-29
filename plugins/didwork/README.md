# DidWork

Independently verify that the outcomes your software claims are actually true.

DidWork receives a claim (`type` + `expected`), gathers evidence from the authoritative system, and returns a verdict: `verified`, `failed`, or `unknown`. This plugin wires the DidWork MCP server into Cursor and teaches the agent to gate on verdicts instead of self-grading.

## Contents

| Piece | Path | Purpose |
| --- | --- | --- |
| MCP config | `mcp.json` | Runs `npx -y @didwork/mcp` with your `DIDWORK_API_KEY` |
| Rule | `rules/verify-outcomes.mdc` | Verify external side effects before reporting success |
| Skill | `skills/verify-outcomes/SKILL.md` | Claim types and the verify → gate workflow |
| Command | `commands/verify.md` | `/verify` a claimed outcome on demand |

## Requirements

- Node.js (for `npx`)
- `DIDWORK_API_KEY` environment variable — get one at [didwork.sh/console](https://didwork.sh/console). `http.ok` claims work without a key.

## Supported claim types

Stripe (refunds, payments, subscriptions, invoices), GitHub (PRs, workflows, issues, releases, deploys, files), GitLab (MRs, pipelines, issues), Linear, Jira, Sentry, email delivery, and `http.ok` for any public URL. Full reference: [didwork.sh/docs](https://didwork.sh/docs).
