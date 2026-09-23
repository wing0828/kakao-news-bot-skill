---
name: kakao-openchat-bot
description: Build, extend, configure, troubleshoot, and document a Korean KakaoTalk Open Chat assistant using MessengerBotR API2, Android notification replies, a Python server, and Gemini-compatible AI. Use when working on this bot architecture or adapting the user's Wonyoung assistant.
---

# Kakao Open Chat Assistant

Use this skill to help build or maintain a KakaoTalk Open Chat bot in the style of the user's Korean AI assistant. This is an installable Codex skill: it provides project guidance, not a running bot, credentials, Android app, or Kakao account. Work against the user's actual project files and verify what is implemented before claiming a feature works.

## Start with the project

- Read the project README, local `AGENTS.md`, dependency manifests, configuration examples, and relevant code before proposing changes.
- Treat `.env`, service keys, OAuth tokens, room IDs, invite links, device IDs, private IPs, and logs as private. Never print or copy them into this skill, docs, issues, or commits.
- If only documentation is requested, do not restart the server, phone app, or bot.
- Preserve a working deployment. Back up files before replacing Android scripts or settings. Do not infer that an app, phone, or server is accessible from this skill alone.
- Read [architecture](references/architecture.md), [commands](references/commands.md), and [safety](references/safety.md) when relevant.

## Architecture to recognize

The reference design has three parts: MessengerBotR API2 on Android receives KakaoTalk notifications and replies; a Windows Python service handles commands, AI requests, scheduled work, and a SQLite outbox; Gemini-compatible APIs provide model responses and optional grounded news search. A local results page may render completed work. The exact project can differ; confirm its README and code before acting.

The phone's notification reply session is stateful. A saved room name or numeric channel ID does not prove the session is currently able to send. For long model requests, prefer immediate acknowledgement, background processing, a durable outbox, and sending from the phone's current received-message context. A delivery acknowledgement is not proof that a person saw the message.

## Working rules

- Identify the precise configured room. Keep an allowlist and compare the identifier supplied by the actual event when available; do not widen access based only on display names or nicknames.
- Keep message intake quick. Do not hold a notification callback open while waiting for a lengthy model or web request.
- Keep model instructions separate from tool permissions. A room-specific persona can change topic and tone, never access control.
- Validate tool input, apply per-user and per-room limits where appropriate, and return useful errors without exposing secrets or stack traces.
- For news and live facts, prefer official sources, verify dates, distinguish confirmed facts from inference, and include direct source links.
- Strip Markdown decoration when the client requires plain text. Respect Kakao message-length limits and split output safely when needed.
- Test health, command parsing, worker/outbox behavior, and error handling locally. For Android delivery, check current compile logs and use an explicit test room. Do not describe local tests as proof of real phone delivery.
- Do not implement nickname-only admin authentication, member removal, account actions, arbitrary shell execution, or unrestricted webhook workflows. Explain platform/API limits and provide a safe alternative.

## Existing-feature orientation

The reference chatbot has included Korean AI news, general Gemini questions, room-specific response profiles, games and simple polls, scheduled reminders/news, weather and selected public-source lookups, document generation, and a local results page. Availability can vary by checkout, API quotas, and external credentials. Check the current code and [feature notes](../../FEATURES.md) before telling users a command is available.

When adding a feature, define its trigger, data source, permissions, output, rate limits, persistence, failure behavior, and a test plan. Prefer a small isolated adapter over adding broad access to the whole server.
