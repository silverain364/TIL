# Docker Compose - 여러 컨테이너를 한 번에 띄우기
앱 + DB + Redis를 명령어 하나로 올리고 내린다. | 2026-05-11

### 개념
실제 서비스는 컨테이너 하나로 돌아가지 않는다. 백엔드 앱, DB, Redis, Nginx가 함께 동작해야 한다.<br/>
Docker Compose는 이 여러 컨테이너를 하나의 yaml 파일로 정의하고, 명령어 하나로 전부 올리고 내리는 도구다.<br/>
로컬 개발 환경 세팅에 특히 강력하다.

### Docker-Compose.yml 기본 구조
```
version : "3.9" services : app # 백엔드 서비스
build : . # 현재 디레토리 Dockerfile 사용
ports : - "8000:8000" env_file : - .env # 환경변수 주입
depends_on : - db - redis db : # PostgreSQL
image : postgres:15 environment : POSTGRES_PASSWORD : secret POSTGRES_DB : mydb volumes : - db-data:/var/lib/postgresql/data # 데이터 영속
redis: image: redis:7-alpine volumes : db-data: # 컨테이너 재시작해도 db 데이터 유지
```

### 자주 쓰는 명령어
`docker compose up -d` : 백그라운드로 전체 서비스 시작.<br/>
`docker compose down` : 전체 서비스 종료 및 컨테이너 삭제.<br/>
`docker compose logs -f app` : 특정 서비스 로그 실시간 확인.<br/>
`docker compose exec app bash` : 실행 중인 컨테이너 안으로 접속.<br/>
`docker compose up --build` : 이미지 다시 빌드 후 시작. 코드 변경 시<br/>

> **depends_on은 순서만 보장** depends_on: db를 써도 DB가 완전히 준비됐다는 보장은 없다. DB가 완전히 뜰 때까지 앱이 재시도하는 로직을 앱 코드에 넣어야 한다.<br/>
> **volmes 핗수** - DB 컨테이너를 내렷다 올리면 데이터가 날아간다. volumes로 호스트에 마운트해야 데이터가 유지된다.
