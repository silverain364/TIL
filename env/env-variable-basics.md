# 환경변수 - 코드에 비밀번호를 절대 쓰지 말기
DB 접속 정보, API 키를 코드에 하드코딩하면 GitHub에 올리는 순간 끝이다. | 2026-04-06

### 개념
환경변수는 코드 밖에서 설정값ㅇ르 주입하는 방법이다. DB 비밀번호, API 키, 포트 번호처럼 **환경마다 달라지거나 노출되면 안 되는 값**을 코드에서 분리할 수 있다.
개발, 스테이징, 운영 환경을 코드 변경 없이 다르게 운영하는 것도 이 덕분이다.


### 하드코딩 vs 환경변수
잘못된 방법
```
DB_PASSWORD = "mypassword123" API_KEY = "sk-adb123xyz"
# GitHub에 올리면 즉시 유출
```
올바른 방법
```
import os DB_PASSWORD = os.getenv("DB_PASSWORD) API_KEY = os.getenv("API_KEY")
# env 파일에서 키를 가져오는 형태로 작성
```

### .env 파일로 관리하기
```
# env 파일(public인 상태에선 절대 git에 올리면 안 됨!)
DB_HOST = localhost DB_PORT = 5432
DB_PASSWORD = mypassword123 API_KEY=sk-abc123xyz
```
```
# .gitignore에 반드시 추가 .env
```
```
# Python에서 .env 로드 (python-dotenv 라이브러리)
from dotenv import load_dotenv
import os load_dotenv() db_password = os.getenv("DB_PASSWORD")
```

### .env.example
```
# .env.example (값은 비워둔 채로 올리기)
DB_HOST=DB_PORT=DB_PASSWORD=API_KEY
```

> **.env는 git에 올리지 말고, .env.example은 올리기.** 팀원이 어떤 환경변수가 필요한지 알 수 있도록 키 목록만 공유하는 게 관례다.
> 실수로 .env를 GitHub에 올렸다면 즉시 해당 키를 폐기하고 재발급해야 된다. git history에 남기 때문에 파일을 지워도 이미 늦는다.
