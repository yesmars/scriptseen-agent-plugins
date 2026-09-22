# ScriptSeen agent publishing

Maintained source: canonical branch `codex/scriptseen-realtime-coach` in the
private application repository. Public distribution is an explicit export to
https://github.com/yesmars/scriptseen-agent-plugins. Never publish the private
repository or copy credentials, customer data, output folders, or deployment
configuration into it.

## Distribution versus approval

The MCP endpoint, downloadable plugin packages, and a self-hosted marketplace
are directly usable. They do not establish a vendor's approval or featured
placement. Record a submission URL/ID before reporting submitted; record the
actual directory entry before reporting listed. See `submissions/STATUS.md`.

## OpenAI: Codex and ChatGPT

Portal: https://platform.openai.com/plugins (Create plugin → With MCP).
Use the universal endpoint https://api.scriptseen.com/mcp and upload the
scriptseen-create skill. Metadata discovery advertises dynamic client
registration, S256 PKCE and authorization-code/refresh flows. A reviewer can
register a public client or client_secret_post/client_secret_basic client.
Do not put a shared personal token in a listing.

The publishing organization needs Apps Management write permission and a
verified individual/business identity. Use the actual verified owner, not an
invented legal organization. Fill from `submissions/listing.json` and
`submissions/test-cases.md`. Supply a dedicated reviewer account with enough
normal quota; never send the owner's credentials. Host the portal-issued exact
verification challenge at `/.well-known/openai-apps-challenge` on the MCP host
or allowed parent host, then Scan Tools. Publish only after approval.

An existing organization can be used even when its internal display name differs
from the public ScriptSeen listing. Do not create or rename an organization just
to match branding. The portal currently blocks **creating** a plugin until
developer identity verification is complete, so the owner must finish that step
before a draft can be prepared in the portal.

Known OAuth scope: this is OAuth, not an OpenID Connect provider. The server
currently does not issue ID tokens or expose UserInfo/openid/email scopes.
OpenAI workspace email-domain restrictions require those additional capabilities;
do not claim support for that enterprise feature. There is no client-id-metadata
URL fetch; dynamic registration is supported instead.

Official requirements checked 2026-09-22:
https://developers.openai.com/plugins/deploy/submission
https://developers.openai.com/plugins/build/auth

## Claude

Public repository: https://github.com/yesmars/scriptseen-agent-plugins.
Its root is the `scriptseen` plugin, with a marketplace of the same name:

    claude plugin marketplace add yesmars/scriptseen-agent-plugins
    claude plugin install scriptseen@scriptseen

Validate with `claude plugin validate <export-directory> --strict`, then submit
at https://platform.claude.com/plugins/submit. Team/Enterprise directory managers
can also use https://claude.ai/admin-settings/directory/submissions/plugins/new.
Accepted applications enter claude-community. claude-plugins-official is curated
separately, with no application process guaranteeing inclusion. Reviewer tests
must include login, refusal, expiry, revocation and the local-media handoff.

The Console form has Introduction, Plugin information and Submission details
steps. Use the public repository root (no subdirectory), select only tested
surfaces, and provide the MIT license, privacy URL and review contact from the
listing kit. Its mandatory checkbox accepts Anthropic's Software Directory
Terms; obtain the owner's explicit action-time confirmation before accepting
through browser automation. A filled form is not a saved draft or submission.
Record the confirmation/receipt after **Submit for review** succeeds.

https://code.claude.com/docs/en/plugins#submit-your-plugin-to-the-community-marketplace

## Grok Build

The public repository root is Claude-compatible and includes skills plus MCP.
Fork https://github.com/xai-org/plugin-marketplace and add one brand-scoped
entry to `.grok-plugin/marketplace.json`, referencing the public repository and
its full 40-character commit SHA. Use category productivity, keyword scriptseen,
and domain scriptseen.com. Do not use generic discovery keywords.

Run their `scripts/generate-plugin-index.py`, `scripts/validate-catalog.py`, and
`generate-plugin-index.py --check`; commit the generated index along with the
catalog entry. Open a PR using their template. Updates bump the SHA through a
new reviewed change. The public ScriptSeen guide links the GitHub repository
to make the relationship between the product and publisher account visible.

https://github.com/xai-org/plugin-marketplace/blob/main/CONTRIBUTING.md
https://docs.x.ai/build/features/skills-plugins-marketplaces

## Grok Bot

Grok Bot's public marketplace distributes Bot templates; it is distinct from
Grok Build's plugin catalog and grok.com's custom connectors. Create a Bot using
`submissions/grok-bot-template.md`, configure the ScriptSeen MCP connection and
test with the owner's own sign-in. Share → Create template → Public link gives
an installable link. Review the template contents: include instructions and
skill, never credentials, personal files, conversation history, or account data.
Recipients connect their own accounts. Public-link sharing does not prove
featured catalog placement. A self-service featured-listing submission route
has not been verified. Team connector policy is managed through Cursor's Team
Marketplace; a Cursor plugin can be submitted separately if needed.

https://docs.x.ai/grok-bot/bots
https://docs.x.ai/grok-bot/teams-and-enterprises
https://x.ai/bot/marketplace

## Muse

Meta's consumer Muse DOES accept connectors: https://muse.ai/platform → Submit
a connector. Describe ScriptSeen using the listing and workflow in this folder.
Meta reviews function, security, legal requirements and the complete workflow;
approval precedes a directory entry. Confirm technical details in their actual
onboarding form instead of assuming a Claude ZIP is a Muse package. Muse Code
is a separate client. The page's public text does not establish a review SLA.

https://muse.ai/platform

## Release and export

1. Test the private source, including OAuth and agent suites; merge/push canonical.
2. Deploy only a clean canonical checkout after the canonical guard.
3. Run `python3 scriptseen/integrations/export_public.py --output <empty-directory>`.
4. Review the exact public files and run plugin/skill validators. Commit/push
   to the dedicated public repository, retaining all existing history.
5. Update submissions with exact immutable SHA and observed results. Do not
   call an untested vendor client compatible just because the protocol tests pass.
