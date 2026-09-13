# 운영 참고

이 문서는 개인 정보를 제거한 운영 사례다. 서버나 휴대폰의 실제 설정을 포함하지 않는다.

## 구성

- 휴대폰: Galaxy Note10+, Android 12, 메신저봇R API2, JavaScript/Rhino.
- 서버: Windows Python 표준 라이브러리 ThreadingHTTPServer, HTTP/JSON, SQLite outbox, 백그라운드 스레드.
- 뉴스: Gemini와 Google Search Grounding. 검색 할당량 오류 시 공식 RSS와 GitHub REST API 수집 후 Gemini 편집으로 대체하는 구성.
- 휴대폰 HTTP 클라이언트: Jsoup.
- 예약: 서버 내부 스케줄러와 휴대폰의 주기적인 대기열 폴링. 예시 운영 주기는 서버 20초 확인, 휴대폰 30초 폴링, 매일 한국 시간 09:00.
- Windows 직접 실행 사례다. Docker, Ubuntu, n8n, Node.js 서버는 이 검증 경로에 포함되지 않는다.

## 배포 전에 로컬에서 확인할 항목

프로젝트 경로, 운영 DB 경로, 서버 주소와 포트, Gemini 모델과 키, 서버 인증값, 허용 방 ID, ADB 장치 ID, 봇 이름, 실행 일정을 실제 환경에서 확인한다. 개인 설정은 이 문서에 채워 넣어 공개하지 않는다.

휴대폰 파일 경로 형식은 /sdcard/msgbot/Bots/<BOT_NAME>/<BOT_NAME>.js이며 로그는 같은 봇 폴더의 log.json과 /sdcard/msgbot/GLOBAL_LOG.json이다. 실제 설치 경로는 확인 후 사용한다.

서버 모듈을 직접 import할 때 기본 DB가 운영 DB와 다를 수 있다. 대기열 확인 시 실행 프로세스의 DB 경로를 명시한다. Windows 시작프로그램 등록은 로그인 후 실행이며, 로그인 없이 부팅만 한 상태의 실행과 다르다.

## 확인된 복구 방식

1. adb devices로 대상을 확인하고 여러 장치가 있으면 명시적으로 선택한다.
2. 기존 코드와 설정을 로컬에 백업한 뒤 필요한 파일만 교체한다.
3. 컴파일 완료 로그를 확인한다. 같은 버튼을 반복해서 누르지 않는다.
4. 실제 허용 방에서 새 알림을 수신해 Message 세션을 확보한다.
5. 뉴스 생성 및 Message.reply 전송과 실제 화면을 확인한다.

정상 작동한 최종 경로는 실제 수신 Message 객체를 보관하고 그 객체의 reply로 대기열 내용을 보내는 방식이다. 직접 bot.send(channelId), 숫자 객체 복원, canReply 값만으로 판단한 방식은 이 환경에서 최종 뉴스 전송을 해결하지 못했다.

## 관측과 한계

- 메신저봇R 0.7.39a에서 시작 화면 정지가 관측됐고 0.7.34a에서 컴파일과 응답이 회복됐다. 특정 장비의 관측이며 모든 환경에 구버전 설치를 권하는 근거는 아니다.
- 최상위 즉시 폴링을 제거한 후 컴파일 완료가 확인됐다. 내부 정지 원인은 확정하지 못했다.
- 화면의 방 제목과 이벤트의 room 값이 달랐다. 실제 이벤트의 channelId를 기준으로 허용 방을 설정했다.
- ping은 오는데 뉴스가 오지 않는 문제는 생성 시간초과와 답장 세션 문제를 각각 구분해 수정했다. 최종 Message.reply 변경 후 사용자 화면에서 뉴스 수신이 확인됐다.
- 앱이나 휴대폰 재시작 후에는 새 수신 알림이 필요할 수 있다. 미래 예약 발송과 무인 재부팅 복구까지 검증한 것으로 확대하지 않는다.
- 알림 관련 시스템 dump의 값 하나가 UI 및 실제 이벤트와 충돌할 때 그 값만으로 권한 문제를 단정하지 않는다.
- RSS 제목만 얻었다면 원문 본문을 읽었다고 하지 않는다. 최근 생성 저장소를 별 개수로 정렬한 결과는 별 증가 속도를 측정한 결과가 아니다.
- 한국어 시험 데이터와 복잡한 인용문은 UTF-8 파일로 다룬다.

## 참고 문서

- [메신저봇R 배포](https://github.com/MessengerBotTeam/msgbot-old-release/releases)
- [공식 알림 파서 변경 안내](https://messengerbotteam.github.io/posts/release-0-7-34.html)
- [API2 Message 참고](https://kbotdocs.dev/reference/api2/Argument/Message)
- [API2 Bot 참고](https://kbotdocs.dev/reference/api2/Object/Bot)

버전이나 API 변경을 실제로 수행할 때는 당시 공식 자료를 다시 확인한다.
