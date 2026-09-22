# ScriptSeen agent plugins

Official ScriptSeen integration source, maintained by the ScriptSeen publisher
under the yesmars account. The product links this repository at
https://scriptseen.com/mcp. This repository contains only the distribution
packages and documentation; ScriptSeen's application and customer data are not
part of it.

Create short-form scripts, paced reference audio, and private browser-video
handoffs through https://api.scriptseen.com/mcp. The root is a Claude/Grok
compatible plugin named `scriptseen`; `plugins/scriptseen-codex` holds the Codex
package. No local executables, hooks, dependency installers or telemetry ship.

## Install

Claude Code:

    claude plugin marketplace add yesmars/scriptseen-agent-plugins
    claude plugin install scriptseen@scriptseen

Use /mcp to sign in to ScriptSeen. Codex:

    codex mcp add scriptseen --url https://api.scriptseen.com/mcp
    codex mcp login scriptseen --scopes scripts:generate,audio:generate,video:prepare

Set tool_timeout_sec = 240 in the Codex server config. You can also install the
included scriptseen-create skill into your client's supported skills directory.
For Grok Build, load the root as a Claude-compatible local plugin or add this
repository as a custom marketplace. Official catalog submission is separate.

Read [INSTALL.md](INSTALL.md) for personal-token fallback and REST clients.
Read [MARKETPLACES.md](MARKETPLACES.md) and [submission status](submissions/STATUS.md)
for public marketplace review routes. Availability is not vendor endorsement.

## Access and privacy

OAuth asks you to sign in at scriptseen.com and approve specific scopes.
Credentials are stored by your MCP client; they never belong in chat or source.
The service stores token hashes, connection metadata and permissions. Access
tokens expire in an hour; refresh tokens rotate and expire with the connection
after 30 days. Revoke at https://scriptseen.com/app/agents.

The plugin contacts api.scriptseen.com for tools and scriptseen.com for login
and private project handoff. Scripts and new reference reads consume the normal
account allowance. The API receives briefs and produces text/audio. Clips and
rendered videos stay on the device running the browser studio. No billing,
coaching, saved-library or account-administration tools are exposed.

Support: https://scriptseen.com/support · Privacy: https://scriptseen.com/privacy
· Terms: https://scriptseen.com/terms. Plugin source is MIT licensed; use of the
hosted service remains subject to its terms.
