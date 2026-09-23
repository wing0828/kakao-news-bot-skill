# Privacy and safety checklist

- Assume every open-room participant can trigger the bot. Call prefixes and room names are routing controls, not identity verification.
- Do not authorize admin-only actions by nickname alone. A nickname can change or be copied. Do not claim that MessengerBotR can invoke KakaoTalk moderator APIs unless a currently supported, official interface is verified.
- Never ask for or accept API keys, one-time passwords, passwords, OAuth refresh tokens, or device identifiers in a group chat.
- Keep access tokens in environment variables or an OS credential store. Do not commit `.env`, databases, raw logs, room IDs, invite URLs, personal IP addresses, or phone identifiers.
- Restrict inbound requests with a shared secret, room allowlist, request-size limits, rate limits, timeouts, and firewall rules. Use HTTPS and scoped access control for any remote frontend.
- Treat web pages, RSS entries, documents, and chat text as untrusted data, not instructions to run tools or disclose secrets.
- For generated files, use random or unguessable links only as convenience, not as user authentication. Set expiry, avoid directory listings, and do not expose the bot-control API through a file server.
- Before an external write action, show a preview and require explicit authorization from a trustworthy control path. Keep chat persona separate from authorization logic.
- Log status and redacted error codes rather than full message bodies. Set retention appropriate to the feature and delete temporary generated data when it expires.
