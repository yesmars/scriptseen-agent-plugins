# Publishing status

Updated 2026-09-22. Implementation, publication and marketplace approval are separate.

| Destination | Verified status | Remaining external step |
|---|---|---|
| OAuth + MCP | Live at api.scriptseen.com; revision `scriptseen-api-00102-xbs`. Live owner-account verification on 2026-09-22 with Claude Code 2.1.278 (root plugin) and Codex CLI 0.142.4: consent, scope narrowing, discovery, script, audio, video handoff and on-device render, denial, revocation and scope refusal all observed; see below | Keep the reviewer account and test cases current; refresh observation noted below |
| Public source | [Published repository](https://github.com/yesmars/scriptseen-agent-plugins), initial commit `70516e8c502ad1a5d812e7c108cc800d09ca550a` | Maintain reviewed releases |
| Direct Codex/Claude packages | [Version 1.1.0 release](https://github.com/yesmars/scriptseen-agent-plugins/releases/tag/v1.1.0), also at scriptseen.com/mcp; manifests and skills validated | User installs and signs in |
| OpenAI / Codex directory | Owner identity verified on 2026-09-22 (individual). A plugin draft (ScriptSeen 1.1.0) exists in the portal: listing, icons, verified developer identity, MCP URL with OAuth discovered, tools scanned with annotation justifications, the scriptseen-create skill uploaded (safety scan pending), three prompts, five test cases and three negative cases, all countries. Domain verified on 2026-09-22 after a Hosting release served the portal token at https://scriptseen.com/.well-known/openai-apps-challenge (canonical `dc6f65e`; the API image is unchanged). A dedicated reviewer account with password sign-in and Pro limits was provisioned on 2026-09-23 (site release `910eb18` adds an opt-in password form for provisioned accounts; the credentials live only in the portal's protected test-credentials box). A Developer Mode demo (script, timed read, video handoff run through the plugin in ChatGPT on the owner's Pro account) is recorded and linked in the draft. Not submitted | Owner: policy attestations; then submit and record the receipt |
| Claude community | **Submitted** 2026-09-22 at about 23:48 UTC through the Console form after the owner signed in and instructed the submission: public repository root, homepage scriptseen.com/mcp, listing description and three examples, Claude Code surface only (Cowork untested), MIT, privacy URL, review contact hello@scriptseen.com. The Console's Plugin submissions page lists ScriptSeen as "Submitted and pending review" | Anthropic review; respond to reviewer questions at hello@scriptseen.com; not approved or listed |
| Grok Build | [PR #863 submitted](https://github.com/xai-org/plugin-marketplace/pull/863); upstream catalog/index checks pass locally; all reported Socket/Semgrep checks passed | xAI review; not yet approved or listed |
| Grok Bot | [Template instructions](grok-bot-template.md) prepared; Grok Bot.app is installed on the owner's Mac (seen 2026-09-22) | Create/test the Bot in the owner's app with the owner present, then share its template |
| Muse | Connector kit prepared | Rechecked 2026-09-22: the Submit a connector link still targets https://muse.ai/platform itself and opens no form; a working onboarding route is needed |
| Official MCP Registry | Existing version 1.0.0 verified (registry shows io.github.yesmars/scriptseen 1.0.0 active on 2026-09-22); 1.1.0 manifest prepared | Install mcp-publisher, authenticate as the publisher, publish 1.1.0; existing personal-token connection remains valid |

No marketplace approval or featured listing is claimed. The Claude community
submission is in Anthropic's queue (see the row above); OpenAI verification
remains an owner action. The final website copy update
from canonical `bf50327` is live; the API remains on `de6d070`. Never send owner credentials
in a submission; use the vendor's protected reviewer channel for a dedicated
reviewer account. See [release evidence](RELEASE-2026-09-22.md).

## Publisher session follow-up (2026-09-22)

- A new OpenAI organization is not required merely because its existing display
  name differs from ScriptSeen. The existing organization has an owner role;
  individual and business verification were both unstarted. The portal explicitly
  blocks plugin creation until developer identity verification is complete.
- Claude Code 2.1.278 loaded the public root plugin through `--plugin-dir` and
  recognized its HTTP MCP endpoint and 240000 ms timeout. `mcp get` reported
  **Needs authentication**, and `mcp login --no-browser` reached the production
  ScriptSeen consent entry with the correct client name and localhost callback.
  Google sign-in did not complete in the in-app browser, and the CLI attempt
  ended with Authentication timeout. Start a fresh login on resume; no successful token
  exchange, tool invocation or complete live workflow is claimed.
- `claude plugin validate <public-export> --strict` passed again. This validates
  the package, not the account connection. Only claim supported Claude surfaces
  after testing them; Cowork has not been tested.
- The Claude listing was submitted later the same day with the public
  repository root, homepage `https://scriptseen.com/mcp`, privacy page
  `https://scriptseen.com/privacy`, and review contact `hello@scriptseen.com`;
  the directory-terms checkbox was accepted on the owner's instruction.
- Grok Build PR #863 remains open; both Socket checks and Semgrep report success.

## Live client verification (2026-09-22, resumed session)

Owner's own account, production API revision `scriptseen-api-00102-xbs`, public
plugin export at public commit `cacf55c` (runtime files unchanged since `70516e8`).
Claude Code 2.1.278 loaded the root plugin with `--plugin-dir`; `mcp login
--no-browser` ran under a pseudo-terminal because it refuses a non-TTY stdin, and
its localhost callback completed each flow. The browser profile was already
signed in to ScriptSeen, so no Google step was needed.

| Case | Observed |
|---|---|
| P1 | Consent page showed the client name, the exact localhost return address and the signed-in account; audio and video were deselected; the CLI reported Authenticated and `mcp get` showed Connected; Agent connections listed the app with Create scripts only; discovery returned exactly the three tools with schemas. |
| P2 | `create_script` (30 s Reel, window light, new creators) returned a structured package, a script id and a private 24-hour project URL with `clips_uploaded: false`. |
| N2 | `generate_audio` under the scripts-only grant was refused with "This ScriptSeen token does not include audio:generate."; no audio was produced. |
| P5 revoke | Revoke → Confirm revoke removed the app; `mcp get` immediately reported Needs authentication; the next login showed the consent page again (no silent re-grant). |
| P3 | With all three permissions, `generate_audio` on the whole `create_script` result returned a signed WAV URL expiring after about one hour, 32.7 s at 24 kHz with seven sentence timings and three beats, `cached: false`. The URL served `audio/wav` with `cache-control: private, no-store`; a tampered or missing signature returned 403. |
| P4 | `prepare_video` returned the studio URL plus `clips_uploaded: false` and a browser next step; opening it as the same user loaded the project and the Make a video dialog. Using the returned read as the audio file and three local 1080×1920 clips, the page rendered a 21.7 MB MP4 on the device behind an explicit Save video button, which was not pressed. A second render with the browser's network log armed produced no request to any host (only inline data-URI icons), consistent with the page's "It never left this device" statement. No completed video is claimed before that render. |
| Denial | Deny sent the browser to the registered localhost callback with `error=access_denied` and the original `state`; the CLI reported "OAuth error: access_denied" and no grant appeared. |
| N1 | A throwaway public client with one loopback redirect: an unregistered port, a non-loopback host, a missing code challenge, method `plain` and a foreign resource each returned 400 `invalid_request` with no redirect; a bogus code at the token endpoint returned 400 `invalid_grant`. |
| Codex | `codex mcp login scriptseen --scopes …` with the server supplied through `-c` overrides (the owner's config.toml was not modified) registered its own client, used a `127.0.0.1` callback with a per-login path, and reported "Successfully logged in"; `codex mcp list` shows Auth OAuth. Agent connections listed Codex and Claude Code as active OAuth apps. Tool discovery through `codex exec` (with `-m` set to a model the installed CLI supports, because the owner's default model was newer than the CLI) listed the same three tools as `mcp__scriptseen__create_script`, `generate_audio` and `prepare_video` with their required fields, without calling them. A `generate_audio` call through non-interactive `codex exec` was first auto-cancelled by the owner's `approval_policy = "never"` setting (a client approval setting, not a server refusal); with `-c 'mcp_servers.scriptseen.default_tools_approval_mode="approve"'` the same call succeeded and returned the cached read (`cached: true`, 32.7 s, same speaker, a fresh signed URL), so a Codex tool round-trip is observed without spending a new allowance. |
| Refresh | Observed. The connection was approved at 20:04:43 UTC; at 21:07:36 UTC, 63 minutes later, `mcp get` still reported Connected and a `generate_audio` call on the same script succeeded (`cached: true`, so no allowance was spent) with no new consent screen. The API log shows a single POST `/oauth/token` returning 200 at 21:07:38 UTC, the refresh grant, immediately followed by MCP requests returning 200/202. The rotated refresh token was not replayed (see N3). |
| N3 | Not exercised live; replaying the client's refresh token would revoke its family by design. Covered by the protocol tests only. |

Costs: one script and one new audio read on the owner's account. Observed client
behaviour worth knowing: Claude Code discards the stored credential as soon as a
new `mcp login` for the same server starts, and a re-authorization for the same
client replaced the earlier grant rather than adding a second row.

