# Docker 레이어 캐시는 순서가 전부다.
  Dockerfile 명령어 순서 하나로 빌드 시간을 10배 줄일 수 있다.

## 날짜
2026-03-30

### 개념
  Docker는 Dockerfile의 각 명령어를 레이어로 저장한다. 한 레이어가 바뀌면 그 아래 모든 레이어를 다시 빌드한다.
  즉, 자주 바뀌는 코드를 위쪽에 두면 매번 전체 빌드가 일어난다.

  FROM python:3.11 COPY . /app # 코드가 바뀌면 아래 전부 재실행 WORKDIR /app RUN pip
  install -r requirements.txt # 매번 설치됨

  FROM python:3.11 WORKDIR /app COPY requirements.txt . # 의존성 파일만 먼저 복사 RUN pip
  install -r requirements.txt # 캐시됨 COPY . . # 코드는 맨 마지막에

  핵심 규칙 : 덜 바뀌는 것을 위에, 자주 바뀌는 것을 아래에. 의존성 파일 → 설정 → 소스코드 순으로!
