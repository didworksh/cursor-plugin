---
name: verify
description: Verify a claimed outcome with DidWork and report the verdict with evidence
---

# /verify

Take the outcome the user describes (or the most recent side-effectful action in this conversation), form the most specific DidWork claim for it, and call the `did_verify` tool.

1. Determine the claim `type` and `expected` fields from context; ask for missing identifiers (payment id, repo/PR number, URL) rather than guessing.
2. Call `did_verify` and report the verdict — `verified`, `failed`, or `unknown` — along with the key evidence fields DidWork returned.
3. If `unknown`, offer to poll with `did_get` or set up a `did_watch`.
