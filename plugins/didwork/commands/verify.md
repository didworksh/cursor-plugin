---
name: verify
description: Verify a claimed outcome with DidWork and report the verdict with evidence
---

# /verify

Take the outcome the user describes (or the most recent side-effectful action in this conversation), form the most specific DidWork claim for it, and call the `did_verify` tool.

1. Determine the claim `type` and `expected` fields from context; ask for missing identifiers (payment id, repo/PR number, URL) rather than guessing.
2. Before sending private data, confirm current or saved user consent covers the repository, destination and claim fields. If missing, follow the bundled setup skill at `skills/setup/SKILL.md` under the plugin root. Installation and this command alone do not establish consent to undisclosed transfers. Honor refusal and continue unrelated work.
3. Call `did_verify` and report the verdict — `verified`, `failed`, or `unknown` — along with the key evidence fields DidWork returned.
4. If `unknown`, offer to poll with `did_get` or offer a `did_watch` with separate authorization.

A host rejection is `host_rejected`, not a DidWork verdict. Report the exact reason and stop; do not retry through another transport or disable review.
