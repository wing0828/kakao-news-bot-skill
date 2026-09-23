# Command design and examples

Call prefixes and exact command syntax are deployment settings. These examples use `원영아`; confirm the active prefix and parser before asking a user to try them.

## Common interactions

- `원영아 도움말` — show only commands enabled in this installation.
- `원영아 뉴스` — fetch and summarize current AI news with dated source links, if the news worker and sources are configured.
- `원영아 서울 날씨 알려줘` — query a configured weather adapter, if present.
- `원영아 퀴즈 시작 5` — start a supported room game, if the game module is installed.
- `원영아 투표 만들기 저녁 메뉴 | 치킨 | 피자` — create an in-bot text poll where supported; this is not KakaoTalk's native poll feature.
- `원영아 알림 30분 회의 시작` — request a reminder where scheduling and phone delivery are configured.
- `원영아 이 내용을 발표용으로 정리해줘: ...` — ask the model to format user-provided content.

These are examples, not universally active commands. Do not invent an enabled command from this list; inspect the current parser/help response. Avoid collecting API keys or private files through a public room. Do not promise reminders across phone restarts until that behavior is explicitly implemented and verified.

## Safe feature specification

For each new command, document:

1. Exact trigger and allowed rooms.
2. Data source and whether user input leaves the local network.
3. Required credentials and minimum scopes.
4. Message format, source links, and output-size behavior.
5. Timeouts, quotas, per-user/room limits, and retry/expiry behavior.
6. What is stored, for how long, and how errors are surfaced.
