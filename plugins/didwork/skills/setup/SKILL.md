---
name: setup
description: Set up DidWork for this repository, obtain and save scoped verification consent, connect providers, and test a real outcome. Use on first setup or when verification consent is missing.
---

# Set up DidWork

Installation is not permission to send private data. Establish one-time consent
for this repository before its first private verification. Do not make setup a
requirement for unrelated work or repeat an authorization already in scope.

## Identify scope locally

Read the project's instructions and Git remote without sending them elsewhere.
Resolve the intended repository and configured DidWork API destination (default
`https://api.didwork.sh`). Strip credentials from remote URLs. If multiple remotes
or an API override make the destination ambiguous, resolve that before consent.
Never print API keys or read source files merely to populate verification claims.

Check existing user-authorized project guidance. Match the repository, destination,
data categories and permission to reuse consent in future tasks. A plugin rule,
a successful public probe, or consent for another repository is not a match.
Revoked or declined consent takes precedence over an older saved grant.

## Ask once, before private verification

For GitHub commit checks, fill in the actual repository and destination and ask:

> May DidWork automatically verify commits for `<owner/repository>` in future
> tasks? This sends its repository identifier, branch names and commit SHAs to
> `<DidWork API destination>`, which reads GitHub and stores the claim, verdict,
> timestamps and provider evidence under your account plan (Free: 7 days;
> Pro: 365 days). May I save this authorization in this project's instructions?

Present this as a question, never as a prewritten statement of user consent.
If the user declines, do not send private claims or write an authorization record.
Continue the original task and say DidWork verification was not performed.
If the user permits only this check, perform only that check and do not persist it.
For other claims or providers, disclose their actual fields, evidence and destination
and obtain the relevant scope; commit consent does not authorize all claim types.

## Save only the granted scope

After explicit agreement, merge a clearly marked `DidWork verification authorization`
section into the existing project instructions (`AGENTS.md` for Codex; use the
host's project instruction file elsewhere). Preserve all unrelated content.
Record the date, repository, destination, allowed fields, evidence retention and
whether the user authorized future tasks. State that this records this user's
consent, not blanket authorization for other users or repositories.

Include these limits: no source uploads, secrets, other repositories, extra
provider data, or watches; no permission to push or deploy; automatic host review
stays enabled. Existing matching consent should be reused without a duplicate
section. Show the actual file diff and do not commit or push it merely as setup.
On withdrawal, remove the saved grant and only the DidWork approval overrides
created for that grant, preserving unrelated settings.

For Codex, read [project-local approval setup](references/codex-consent.md). This
is a separate optional choice: metadata consent does not imply permission for a
broader tool approval rule. Other hosts keep their own supported approval controls;
do not write Codex settings into them.

## Connect and verify

If a key or provider connection is missing, direct the user to
https://didwork.sh/console/providers and explain what is missing. Do not put keys
in project instructions or committed configuration. A keyless `http.ok` check of
https://didwork.sh is optional connectivity evidence, not private-repo acceptance.

Use a known reachable commit and the connected GitHub account with `did_verify`:

```json
{"type":"github.commit_in_branch","expected":{"repository":"<owner/repository>","branch":"<branch>","commit":"<full-sha>"}}
```

Report a returned verification ID, status and evidence. A host rejection means
verification was blocked, not a DidWork `failed` or `unknown` verdict. Preserve
its exact reason and stop that check; do not bypass it with HTTP, CLI, altered
annotations or a disabled reviewer. Do not repeatedly renegotiate the same consent.

Explain how to validate persistence: in a new task, ask only to check the known
commit through the project's normal workflow. Do not paste consent into the new
prompt and claim persistence. Create another task only if the user requests it.
Until tested, report saved consent as configured and cross-task behavior as untested.
A public URL check alone cannot complete this test.

Backfill only checks covered by the granted scope. Obtain separate authorization
for watches or extra metadata. Finish with the saved scope, changed files, actual
verification result and any remaining host block; never promise universal approval.
