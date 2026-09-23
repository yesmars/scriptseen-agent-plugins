# ScriptSeen agent setup

MCP endpoint: https://api.scriptseen.com/mcp (Streamable HTTP).
Connect with OAuth: sign in on scriptseen.com, review the client return address,
select permissions, and choose Allow connection. The agent receives its own
credentials, never your Firebase login. Access tokens last one hour; rotating
refresh tokens and the connection expire after 30 days. Revoke in
https://scriptseen.com/app/agents. Discovery is free; script generation and new
reference reads use your normal ScriptSeen plan allowances.

## Codex

    codex mcp add scriptseen --url https://api.scriptseen.com/mcp
    codex mcp login scriptseen --scopes scripts:generate,audio:generate,video:prepare

Set tool_timeout_sec = 240 under [mcp_servers.scriptseen] in your config.toml.
Download https://scriptseen.com/mcp/scriptseen-codex.zip for the MCP + skill
package. The source repository is https://github.com/yesmars/scriptseen-agent-plugins.
This is direct distribution, not an approved OpenAI directory listing.

## Claude Code

    claude plugin marketplace add yesmars/scriptseen-agent-plugins
    claude plugin install scriptseen@scriptseen

Then use /mcp to connect ScriptSeen and approve permissions in your browser.
Alternatively download https://scriptseen.com/mcp/scriptseen-claude.zip,
extract it, and run claude --plugin-dir /absolute/path/to/scriptseen-claude.
Direct remote configuration:

    {"mcpServers":{"scriptseen":{"type":"http","url":"https://api.scriptseen.com/mcp","timeout":240000}}}

## Grok, Grok Build, and Grok Bot

Grok: https://grok.com/connectors → New Connector → Custom, then add the MCP
URL and authenticate. Grok Build accepts Claude-compatible plugins and custom
marketplace sources; the public repository includes the skill and MCP config.
Official Grok Build catalog review is separate from direct installation.
Grok Bot shares can distribute a configured Bot template; each recipient must
connect their own ScriptSeen account. Shared templates must not contain tokens.
Grok Bot team connectors use the team's Cursor connector policy; do not assume
that adding a connector on grok.com installs it into Grok Bot.

## Muse

Meta accepts connector submissions at https://muse.ai/platform. A submitted
connector is not usable from its directory until Meta approves it. ScriptSeen's
submission status and exact client testing are documented in the source repo.
Until listed, use https://scriptseen.com/app in Muse's browser. The browser is
on Muse's cloud computer: clips there are not automatically on your phone.
Muse Code is a different client; use its documented MCP support if applicable.

## Personal-token fallback

For clients without OAuth, create a token with only the needed scopes at
https://scriptseen.com/app/agents. Store it in the client's secret environment,
not chat or source control. Send Authorization: Bearer <token> only to the
ScriptSeen API. For Codex add --bearer-token-env-var SCRIPTSEEN_AGENT_TOKEN
instead of running mcp login. For Claude add a headers object containing
"Authorization": "Bearer ${SCRIPTSEEN_AGENT_TOKEN}" to the server configuration.
Personal tokens remain active until revoked. Use one per client.

## REST fallback

Use the same bearer credentials and application/json:
- GET https://api.scriptseen.com/v1/agent/capabilities (free discovery: tools,
  write modes, and the voices and languages audio accepts).
- POST https://api.scriptseen.com/v1/agent/scripts with
  {"topic":"a useful filming tip","audience":"new creators","platform":"reels","duration":30}.
- POST https://api.scriptseen.com/v1/agent/audio with result.package under
  "package", result.request under "request", and result.script_id under
  "script_id". Optional "speaker" and "language" select the read.
- POST https://api.scriptseen.com/v1/agent/video-projects with
  {"project_url":"<the exact project.studio_url returned by script creation>"}.
Credentials never authorize account, coaching, billing, or saved-library routes.

## Workflow and errors

Discover tools, then call create_script. Pass its complete result to
generate_audio. Pass project.studio_url to prepare_video. That final tool
returns a private browser handoff, not an MP4. Open it signed into the SAME
ScriptSeen account on the device holding the clips, select them and render.
Clips and finished videos are not uploaded to the ScriptSeen API.
Project links last 24 hours; audio links also expire. Keep them private.
A 401 means absent, expired or revoked credentials. A 403 means missing scope
or unapproved browser Origin. On 429, wait for the allowance/rate-limit window.
Do not blindly retry generation after a timeout: it may already have completed.

## Publishing

See MARKETPLACES.md in the public source repository for publishing links,
submission material, verification requirements and status. Downloadable plugins
and MCP Registry publication are not marketplace approvals.
