# Codex project approval policy

A saved authorization record communicates the user's actual consent to future
tasks. Tool settings influence host approval handling; neither guarantees that
Codex's separate automatic reviewer will approve a call.

Before suggesting changes, inspect the project's `.codex/config.toml` and the
installed plugin/server identities. Inspect user-level configuration only as
needed to identify a conflicting standalone server, and never display credentials.
An old standalone `mcp_servers.didwork` can shadow the plugin. Do not delete,
disable, or migrate it or its credentials without the user's authorization.

Offer the following separately from saving consent:

> May I also enable project-local automatic approval for DidWork's `did_verify`
> tool? Codex's setting is per tool, not per repository argument: it can cover
> any `did_verify` claim made while working in this project. Your recorded data
> authorization will remain limited to the repository and fields you approved.
> Other tools and the automatic reviewer will keep their existing settings.

If declined, retain only the authorized consent record and normal host approval.
If accepted, show the concrete proposed diff, then merge it without overwriting
unrelated settings. Resolve existing entries rather than append duplicate TOML
headers. Never change global approval defaults or reviewer settings.

Use the actual installed plugin ID and server name; `didwork@didwork` and
`didwork` are examples from the DidWork marketplace, not universal identifiers:

```toml
[plugins."didwork@didwork".mcp_servers.didwork]
enabled = true
default_tools_approval_mode = "prompt"

[plugins."didwork@didwork".mcp_servers.didwork.tools.did_verify]
approval_mode = "approve"
```

Write this only to the project `.codex/config.toml`. Preserve existing policies
on all other tools. If these entries conflict with an existing server policy,
resolve the specific diff with the user instead of silently widening it. Do not
invent standalone-server settings from plugin-only documentation.

Validate TOML with an available parser and confirm the intended server/tool is
exposed. Unsupported configuration, an unavailable tool, a host rejection, and
a DidWork verdict are distinct outcomes. Do not repeatedly try equivalent calls
when a host rejection has occurred. If the automatic reviewer still rejects the
recorded consent, retain the sanitized reproduction for host support.

Official reference:
https://developers.openai.com/plugins/build/plugins#bundled-mcp-servers-and-lifecycle-hooks
