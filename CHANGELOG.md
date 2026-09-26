# 0.6.0 — 2026-09-26

The setup skill explains private-data transfers and evidence retention, asks for
repository-specific authorization, and saves consent only after agreement.
Verification entry points reuse matching consent, honor refusal, and report host
rejections separately from DidWork verdicts. Project-local Codex tool approval is
an optional separate choice and does not disable automatic review.

Local acceptance used a newly installed plugin with MCP 0.1.1, an existing
DidWork/GitHub connection, a private repository, and Codex automatic review.
Setup asked before verification. Refusal produced no claims or saved consent.
After one scoped authorization, a fresh task verified a known commit without a
new consent prompt. A separate task pushed a temporary branch and automatically
verified it using saved consent and project-local tool policy. The temporary
branch was removed afterward. This validates the tested local Codex workflow;
it does not establish new-account provider onboarding or every host/approval mode.

The shared consent workflow was exercised through Codex. Cursor packaging and
skill validation passed; a live Cursor session was not part of this local run.
The npm package remains 0.1.1.
