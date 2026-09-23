# Agent OAuth operations

Issuer: https://api.scriptseen.com. Resource/audience:
https://api.scriptseen.com/mcp. Production uses Firestore; local tests use
MemoryStore with the same transaction callback contract. Firebase remains the
human identity provider and is used only by ScriptSeen's consent page.

## Protocol

- Discovery: `/.well-known/oauth-authorization-server` and
  `/.well-known/oauth-protected-resource/mcp` (also root resource metadata).
  MCP 401 responses carry the resource_metadata WWW-Authenticate challenge.
- POST `/oauth/register`: JSON `client_name`, exact `redirect_uris`, optional
  `token_endpoint_auth_method` (`none`, `client_secret_post`, `client_secret_basic`).
  Secret shown once for confidential clients, hashed at rest. Client registration
  expires after one year; register again if invalid_client then occurs.
- GET `/oauth/authorize`: client_id, redirect_uri, response_type=code,
  code_challenge_method=S256, code_challenge, scope, resource and state.
  Redirects to `/app/connect#request=...`; requires an HttpOnly Secure host-only
  SameSite=Lax flow cookie and expires after ten minutes. An unknown client or an
  unregistered callback gets a 400 page and is never redirected. Once both check
  out, any other refusal (`unsupported_response_type`, `invalid_request`,
  `invalid_target`, `invalid_scope`) returns to a loopback callback with
  `error`, `error_description` and `state`; an https callback gets the 400 page,
  because open registration means it proves nothing (no open redirector,
  RFC 9700 §4.11.2).
- GET `/oauth/request/{id}` requires that flow cookie. It returns only the
  client name, client_id, callback and requested scopes for the consent page.
- POST `/oauth/consent`: JSON request_id, approve, scopes; exact site Origin,
  Firebase login and the flow cookie required. The consent page signs in with
  Google; provisioned reviewer and test accounts use its password form instead
  (no self-serve password sign-up). No automatic authorization.
  Rejects added scopes. Returns a validated callback URL, preserving state.
- POST `/oauth/token`: form-encoded authorization_code or refresh_token grant.
  Code is single-use, two-minute lifetime, client/redirect/resource/S256-bound.
  Access is an opaque `ss_oauth_` token, valid up to one hour. Refresh is opaque
  `ss_refresh_`, rotates atomically, expires at the original 30-day grant end.
  A failed Basic client authentication answers 401 with `WWW-Authenticate: Basic`.
  Reusing a consumed code/refresh token revokes its entire family while the
  consumed record remains retained. Never retry a lost refresh response blindly;
  restart authorization because replay is deliberately fail-closed.
- POST `/oauth/revoke`: form token + client authentication. Revokes its family;
  unknown tokens return success without disclosing whether another client owns it.
- GET/DELETE `/v1/agent/connections[/grant-id]`: Firebase-only owner management.
  Ten simultaneous OAuth connections per account, separately from personal tokens.

OAuth credentials never authorize account, billing, coaching or saved-library
routes. The same scoped agent identity and quotas protect MCP and agent REST.
Every tool request rechecks the grant and account, so revocation is immediate
for subsequent calls. It does not cancel a generation already admitted.

## Security and retention

Record collection `scriptseen_oauth`; secrets only in SHA-256 document IDs or
hash fields. Grant indexes contain account ID, client label, scopes and expiry.
Expired records carry Firestore `expires_at` TTL. Expiration is enforced in
application code independently of asynchronous TTL deletion. Account deletion
removes that account's grants, flow records and tokens. Public client metadata
has its own one-year expiry. Per-IP and global hourly admission counters bound
registration, authorization, token and revocation requests; never clear those
counters to force tests through. The shared counters use Firestore transactions.

No client-metadata URL fetching, dynamic resource targets, wildcard redirects,
implicit/password/client-credentials grant, or auto-consent. HTTP redirects are
permitted only to literal loopback addresses/localhost for native client callbacks.
Production flow cookies require HTTPS, with website and API on the same site.
For local UI tests use an HTTPS proxy or Playwright's isolated mocked fixture;
do not weaken production cookie security to make HTTP localhost work.

This is OAuth, not OpenID Connect: no ID tokens, UserInfo, openid/email scopes,
or enterprise email-domain restrictions. DCR is advertised; client metadata
documents are not. Document these boundaries in submissions.

## Tests and release

`venv/bin/python -m unittest tests.test_scriptseen_oauth tests.test_scriptseen_agent_api tests.test_scriptseen_store_parity`
then all checks required by the main ScriptSeen skill. Browser flow:
authorization URL → account sign-in → deselect permission → explicit approval
or denial → registered callback; verify reconnect and revocation.

Apply the OAuth TTL field from `deploy/gcp/recovery.tf` with the normal guarded
canonical deployment. Never use a Firebase ID token as a marketplace demo secret.
Supply reviewer credentials only through the publisher's protected portal.

Sources: https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
and https://developers.openai.com/plugins/build/auth (checked 2026-09-22).
