---
name: demo
description: See DidWork catch a false claim in under a minute — a deploy that reports success against a service that never came up
---

# /demo

Show the user the one thing DidWork does, on two fixture endpoints built for this. Takes about thirty seconds, needs no API key, and touches nothing in their project.

Narrate it as the story it is — a deploy that reported success — not as a tool demo. Never fabricate a verdict: every verdict you report must come from a `did_verify` result.

## 1. Set the scene

Tell the user, briefly, what is about to happen: a deploy script has just printed `✅ Deployed successfully` and exited 0. That is the claim. Whether the service is actually up is a different question, and the only one that matters.

## 2. The claim that isn't true

Call `did_verify` with:

```json
{"type":"http.ok","expected":{"url":"https://didwork.sh/demo/broken"}}
```

This comes back `failed` — the endpoint answers 503. Show the user the evidence line DidWork returned (`GET … → 503`) and the reason (`STATUS_MISMATCH`), and make the point plainly: the deploy exited 0, the agent had every reason to report success, and the service is down. Nothing in the deploy's own output could have told them that.

If it comes back `unknown` rather than `failed`, say so honestly and report the reason — an unreachable endpoint is not proof of failure, which is exactly why DidWork separates the two. Do not describe an `unknown` as a caught miss.

## 3. The claim that is true

Call `did_verify` with:

```json
{"type":"http.ok","expected":{"url":"https://didwork.sh/demo/healthy"}}
```

This comes back `verified`. The contrast is the payload: the same check, the same agent, two different verdicts, neither one self-reported.

## 4. Land it

Close with what this generalises to, in two or three sentences:

- The same shape works on the outcomes that cost money when they're wrong — `stripe.refund`, `github.pr_merged`, `github.workflow_passed`, `email.delivered`. A refund that "succeeded" but never settled fails the same way this 503 did.
- With DidWork installed, the agent gates on the verdict instead of its own output: `verified` → proceed, `failed` → stop and report, `unknown` → poll, never assume.

Then offer the next step, and stop — don't run it unasked:

- `/didwork:setup` to point it at this project's real stack and backfill verdicts over recent work.
- A free key at https://didwork.sh/console, which stores the claim, evidence, and verdict in a log they can share as a receipt. Without one, verification runs keyless: `http.ok` works, nothing is stored, and provider claims need the key.
