# Publishing status

Updated 2026-09-22. Implementation, publication and marketplace approval are separate.

| Destination | Verified status | Remaining external step |
|---|---|---|
| OAuth + MCP | Live at api.scriptseen.com; revision `scriptseen-api-00102-xbs` | Complete live vendor-client login with the owner's account |
| Public source | [Published repository](https://github.com/yesmars/scriptseen-agent-plugins), initial commit `70516e8c502ad1a5d812e7c108cc800d09ca550a` | Maintain reviewed releases |
| Direct Codex/Claude packages | Version 1.1.0 available at scriptseen.com/mcp; manifests and skills validated | User installs and signs in |
| OpenAI / Codex directory | Submission kit prepared; portal opens a login screen | Owner sign-in, publisher identity, domain challenge, reviewer account and review |
| Claude community | Self-hosted marketplace available; Console submission opens a login screen | Owner sign-in, submission and review |
| Grok Build | [PR #863 submitted](https://github.com/xai-org/plugin-marketplace/pull/863); upstream catalog/index checks pass locally | xAI review and CI; not yet approved or listed |
| Grok Bot | [Template instructions](grok-bot-template.md) prepared | Mac was locked; create/test the Bot in the owner's app and share its template |
| Muse | Connector kit prepared | The visible Submit a connector link points to /platform and opens no form; a working onboarding route is needed |
| Official MCP Registry | Existing version 1.0.0 verified; 1.1.0 manifest prepared | Publisher authentication and version update; existing personal-token connection remains valid |

No marketplace approval or featured listing is claimed. OpenAI and Claude
publisher tabs are preserved for account sign-in. The final website copy update
from canonical `bf50327` is live; the API remains on `de6d070`. Never send owner credentials
in a submission; use the vendor's protected reviewer channel for a dedicated
reviewer account. See [release evidence](RELEASE-2026-09-22.md).
