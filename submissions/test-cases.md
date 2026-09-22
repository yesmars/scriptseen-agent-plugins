# Reviewer test cases

Use a dedicated ScriptSeen account. Never use a shared production owner token.
These are expected outcomes, not assertions that every vendor has passed them.

| ID | Input / action | Expected result |
|---|---|---|
| P1 | Connect the MCP URL, sign in and approve scripts only; discover tools. | Consent shows account, client callback and requested permissions. Free discovery succeeds; generation scopes stay limited. |
| P2 | “Write a 30-second Reel about filming with window light for beginners.” | create_script returns a structured script and private owner-only project URL; script quota applies. |
| P3 | Connect with audio permission, then “Generate the reference read for that script.” | generate_audio receives the whole script result and returns an expiring audio URL plus timing. A new read uses allowance. |
| P4 | Connect with video permission; “Prepare a video from that script.” | prepare_video returns a browser URL. Open as the same user, select local clips, render MP4. No claim of a completed video before rendering. |
| P5 | Let the access token expire, refresh once, then revoke in Agent connections. | Refresh rotates the token; revoked access and refresh stop working. Reconnection requires approval. |
| N1 | Alter the redirect URI, resource or PKCE verifier during login/exchange. | Rejected; no authorization code sent to an unregistered callback and no usable credentials issued. |
| N2 | Request audio without audio permission, or access billing/account routes with OAuth. | Refused without generating audio or changing account/billing. |
| N3 | Replay a consumed refresh token or use another account's private project link. | Replay revokes its credential family. Cross-account project recovery is refused. |

Also verify denial returns access_denied and preserves state; UI errors are
readable; cancellation does not grant access; 320px mobile layout is usable;
no credentials appear in screenshots, analytics, logs, or plugin packages.
Generation is not idempotent: a timeout must not cause an automatic duplicate.
