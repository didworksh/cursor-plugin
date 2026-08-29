# DidWork for Cursor

**Your agent said it's done. Did it work?**

This repository packages [DidWork](https://didwork.sh) as a Cursor plugin. DidWork independently verifies that the outcomes software claims to have produced are actually true: it receives a **claim**, gathers **evidence** from the authoritative system (Stripe, GitHub, GitLab, Linear, Jira, Sentry, email, any HTTP endpoint), and returns a **verdict** — `verified`, `failed`, or `unknown` — that your agent can gate on instead of grading its own work.

## What's in the plugin

- **MCP server** — `did_verify`, `did_get`, `did_list`, `did_watch`, `did_watches`, `did_unwatch`, `did_usage`, provided by the published [`@didwork/mcp`](https://www.npmjs.com/package/@didwork/mcp) package (run via `npx`, nothing vendored here).
- **Rule** (`verify-outcomes`) — nudges the agent to verify external side effects before reporting success.
- **Skill** (`verify-outcomes`) — the full workflow and claim-type reference.
- **Command** (`/verify`) — verify a claimed outcome on demand.

## Setup

Install the plugin, then set `DIDWORK_API_KEY` in your environment (get a key at [didwork.sh/console](https://didwork.sh/console)). `http.ok` claims — "is this URL healthy?" — work with no key at all:

```sh
curl -s https://api.didwork.sh/v1/verify \
  -H 'content-type: application/json' \
  -d '{"type":"http.ok","expected":{"url":"https://your.app/health"}}'
```

## About this repository

This repo contains only the plugin packaging: manifests, rules, skills, and the MCP configuration. The DidWork verification service itself is a hosted API at `https://api.didwork.sh`; the client SDKs are published as [`@didwork/sdk`](https://www.npmjs.com/package/@didwork/sdk) (TypeScript) and `didwork` (Python).

Docs: https://didwork.sh/docs · Contact: https://didwork.sh

## License

The contents of this repository (plugin configuration and documentation) are MIT licensed — see [LICENSE](LICENSE). The DidWork hosted service is a separate commercial offering and is not licensed by this repository.
