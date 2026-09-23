# Reference architecture

This guide describes a pattern, not a promise that every installation has these files or exact behavior.

## Message path

1. Android MessengerBotR API2 receives a KakaoTalk message notification.
2. The phone applies the configured room allowlist and call prefix, then sends an authenticated HTTP request to the Python service.
3. The service answers short commands quickly. Longer work returns an acknowledgement and runs in a background worker.
4. Results are stored in SQLite until the phone can send them through a valid KakaoTalk notification reply context.
5. The phone reports send success and the service acknowledges the outbox item. A crash between send and acknowledgement can cause a duplicate.

## Runtime and operations

- Windows direct service and Docker/WSL are alternative deployments. Avoid running both on the same ports or database.
- Keep service secrets in local environment configuration. Provide `.env.example` with empty or obviously fake values only.
- Bind the service to the required interface; firewall it to the trusted LAN. Do not expose a bot-control endpoint directly to the public internet. A read-only results frontend should be isolated from command and send endpoints.
- Use a bounded queue, request timeouts, retries with expiry, and visible health status. Scheduled work depends on the host and phone being online and the phone's reply session being available.
- Persist only what the feature needs. Keep chat transcripts out of logs by default; redact authorization headers and sensitive request fields.

## AI and data sources

Gemini may be called through its native API or a configured OpenAI-compatible endpoint. Model names, quotas, and API behavior change; read the project's current configuration and vendor documentation before changing them. For news, a fallback can gather official RSS/GitHub sources before asking the model to synthesize; label the sources actually fetched and do not imply that a headline was full-text verified.

External tools such as weather, public GitHub lookup, document creation, or a local result viewer are optional adapters. Only enable integrations that are present in the checkout and whose credentials and scopes are configured intentionally.
