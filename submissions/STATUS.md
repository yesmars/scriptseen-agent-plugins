# Publishing status

Updated 2026-09-22. Implementation, publication and marketplace approval are separate.

| Destination | Verified status | Remaining external step |
|---|---|---|
| OAuth + MCP | Live at api.scriptseen.com; revision `scriptseen-api-00102-xbs` | Complete live vendor-client login with the owner's account |
| Public source | [Published repository](https://github.com/yesmars/scriptseen-agent-plugins), initial commit `70516e8c502ad1a5d812e7c108cc800d09ca550a` | Maintain reviewed releases |
| Direct Codex/Claude packages | [Version 1.1.0 release](https://github.com/yesmars/scriptseen-agent-plugins/releases/tag/v1.1.0), also at scriptseen.com/mcp; manifests and skills validated | User installs and signs in |
| OpenAI / Codex directory | Publisher signed in with an existing owner-managed organization; Create plugin → With MCP requires verified identity before even creating a draft | Owner completes individual/business identity verification, then domain challenge, reviewer account, submission and review |
| Claude community | Publisher signed in; repository, listing, MIT license, privacy URL and review contact entered in the Console form; not submitted | Complete live client sign-in/test, accept directory terms with owner confirmation, submit and verify receipt |
| Grok Build | [PR #863 submitted](https://github.com/xai-org/plugin-marketplace/pull/863); upstream catalog/index checks pass locally; all reported Socket/Semgrep checks passed | xAI review; not yet approved or listed |
| Grok Bot | [Template instructions](grok-bot-template.md) prepared | Mac was locked; create/test the Bot in the owner's app and share its template |
| Muse | Connector kit prepared | The visible Submit a connector link points to /platform and opens no form; a working onboarding route is needed |
| Official MCP Registry | Existing version 1.0.0 verified; 1.1.0 manifest prepared | Publisher authentication and version update; existing personal-token connection remains valid |

No marketplace approval or featured listing is claimed. OpenAI verification and
the prepared Claude submission tabs are preserved for owner action. Form contents
are not a confirmed saved draft or submission receipt. The final website copy update
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
  Google sign-in did not complete in the in-app browser; no successful token
  exchange, tool invocation or complete live workflow is claimed.
- `claude plugin validate <public-export> --strict` passed again. This validates
  the package, not the account connection. Only claim supported Claude surfaces
  after testing them; Cowork has not been tested.
- The prepared Claude listing uses the public repository root, homepage
  `https://scriptseen.com/mcp`, privacy page `https://scriptseen.com/privacy`,
  and review contact `hello@scriptseen.com`. The mandatory directory-terms
  checkbox remains unchecked pending explicit owner confirmation.
- Grok Build PR #863 remains open; both Socket checks and Semgrep report success.
