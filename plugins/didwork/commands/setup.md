---
name: setup
description: Set up DidWork with one-time repository consent and a real verification check
---

Use the bundled DidWork `setup` skill at `skills/setup/SKILL.md` under this
plugin's root. In hosts that expose it, resolve `PLUGIN_ROOT` or
`CLAUDE_PLUGIN_ROOT`; do not resolve that path relative to a generated command
wrapper. If the skill cannot be found, report the missing component rather than
run private verification without its consent flow.

Start by identifying the repository and checking existing scoped authorization.
Explain the data transfer and obtain explicit agreement before saving consent.
Installing DidWork or invoking this command does not itself authorize the transfer.
