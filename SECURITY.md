# Security

The full security description of the RegAI Legal MCP is published at
**https://regai.tw/mcp/security**. In short:

- **Read-only, public data only.** The tools search public Taiwan laws
  (Ministry of Justice) and court decisions (Judicial Yuan). They never change
  data, never call other outside services, and have no access to RegAI web-app
  documents, chat history or account data. Every tool is annotated
  `readOnlyHint: true`, `destructiveHint: false`.
- **Indirect prompt injection.** Tools return official legal text, and the
  server tells agents that returned text is material to cite, not
  instructions. Keep "confirm before running tools" on in your assistant.
- **API keys.** One key per account (`regai_live_…`, 256 random bits), matched
  by hash and stored encrypted (AES-256-GCM) so you can view it again.
  Regenerate or revoke it any time at https://regai.tw/account. Prefer the
  `Authorization: Bearer` header over the `?key=` URL parameter.
- **Sign-in (OAuth).** Clients that support it connect by signing in on
  regai.tw (OAuth 2.1, PKCE S256, exact redirect matching, tokens bound to
  this service). Access tokens last 1 hour and are accepted only in the
  `Authorization` header; refresh tokens last 90 days and rotate on every use
  (reuse of an old one revokes the app). Disconnect apps any time under
  "Connected apps" at https://regai.tw/account.
- **Transport and limits.** HTTPS only; rate limits and monthly quotas per
  account.
- **Logging.** Time, tool and count of calls for billing and quotas; no query
  content, only a keyed fingerprint for troubleshooting. See
  https://regai.tw/privacy.

## Reporting a vulnerability

Email **security@regai.tw**. Please don't open a public issue for security
problems.
