# Kakao News Bot Skill

카카오톡 AI 뉴스봇을 유지보수하고 복구하기 위한 한국어 Codex 스킬입니다. 메신저봇R API2 + Windows Python 서버 + Gemini 구성의 실제 운영에서 확인한 진단 방법을 정리했습니다.

이 저장소는 **운영 스킬과 참고 문서**를 제공합니다. 실행용 봇 서버와 휴대폰 소스 전체는 포함하지 않습니다.

## 포함 내용

- 방 이름 불일치와 알림 이벤트 진단
- 뉴스 생성 시간초과를 피하는 비동기 outbox 구조
- 실제 수신 Message 객체의 reply를 사용하는 전송 방식
- 재시작 후 발송 세션과 예약 전송의 한계
- 한국어 일반 텍스트 뉴스 및 출처 확인 기준

## 설치

저장소를 내려받아 SKILL.md와 references 폴더가 함께 있는 디렉터리를 Codex의 개인 skills 디렉터리에 kakao-news-bot 이름으로 복사합니다.

Windows PowerShell 예시:

```powershell
git clone https://github.com/wing0828/kakao-news-bot-skill.git
$skillsRoot = Join-Path $env:USERPROFILE '.codex\skills'
$skillTarget = Join-Path $skillsRoot 'kakao-news-bot'
if (Test-Path $skillTarget) { throw '기존 스킬을 백업하고 내용을 비교한 뒤 설치하세요.' }
New-Item -ItemType Directory -Path $skillTarget -Force | Out-Null
Copy-Item .\kakao-news-bot-skill\SKILL.md $skillTarget
Copy-Item .\kakao-news-bot-skill\references $skillTarget -Recurse
```

별도 CODEX_HOME을 사용하는 환경은 해당 위치의 skills 디렉터리를 사용합니다. 설치 후 새 작업에서 스킬 목록을 확인하고 다음처럼 요청하세요.

> $kakao-news-bot 뉴스봇은 답장하지만 뉴스가 오지 않는 원인을 확인해줘.

## 기술 구성

| 역할 | 기술 |
| --- | --- |
| 카카오톡 알림 수신·답장 | Android, 메신저봇R API2, JavaScript/Rhino |
| 휴대폰 ↔ 서버 | Jsoup, HTTP/JSON |
| 뉴스 생성 | Python, Gemini, Google Search Grounding |
| 대체 자료 수집 | 공식 RSS, GitHub REST API |
| 발송 대기열 | SQLite |
| 예약 실행 | Python 스케줄 확인, 휴대폰 타이머 |

이 스킬만 설치하면 봇이 자동으로 설치되거나 실행되는 것은 아닙니다. 서버·휴대폰 코드와 로컬 설정은 별도로 필요합니다.

## 공유 및 보안

API 키, 인증 토큰, 채팅방 이름·ID·초대 링크, 닉네임, 장치 일련번호, 개인 IP, 운영 로그 및 DB는 포함하지 않습니다. 개인 설정을 문서에 추가했다면 다시 공유하기 전에 제거하세요.

카카오 또는 메신저봇R의 공식 프로젝트가 아닙니다. MIT 라이선스는 이 저장소의 문서와 스킬에 적용되며 외부 앱과 서비스에는 각자의 조건이 적용됩니다.

## 라이선스

[MIT](LICENSE)

## 추가로 설치할 수 있는 챗봇 스킬

`skills/kakao-openchat-bot`에는 MessengerBotR API2 + Python + Gemini 기반의 카카오톡 오픈채팅 챗봇을 만들고 유지보수하는 Codex 스킬이 있습니다. 아키텍처, 명령 설계, 개인정보·보안 참고자료를 포함하며 실행 가능한 봇 코드나 인증 정보는 포함하지 않습니다.

설치하려면 `skills/kakao-openchat-bot` 폴더 전체를 Codex skills 폴더에 복사하세요. [스킬 진입점](skills/kakao-openchat-bot/SKILL.md)을 참고하세요.

관련 문서: [챗봇 기능 안내](FEATURES.md), [Buzz 적용 아이디어](BUZZ-IDEAS.md).
