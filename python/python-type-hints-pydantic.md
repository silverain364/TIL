# Python 타입 힌트 - 코드를 문서처럼 읽히게 만들기
6개월 후에 나도, 동료도 타입만 봐도 뭘 넣어야 하는지 안다. | 2026-07-29

### 개념
Python은 동적 타입 언어라 변수에 어떤 타입이 와야 하는지 강제하지 않는다. 타입 힌트는 강제는 아니지만 IDE 자동 완성·오류 감지·코드 가독성을 크게 높인다.
FastAPI는 타입 힌트를 기반으로 유효성 검사와 API 문서를 자동 생성하기도 한다.

### 타입 힌트 없음 VS 있음
```
def create_user(name, age, email):...
//name이 str? int?
//age가 없어도 되나?
//리턴값은?
```

```
def create_user( name: str, age: int, email: str) -> dict: ...
//명확하게 읽힘
```

> **Option[str]은 str|None의 줄임** "이 값은 없을 수도 있다."는 표현이다. DB에서 NULL이 될 수 있는 컬럼과 대응된다고 생각하면 쉽다.</br>

> **FastAPI와 찰떡궁합** FastAPI는 Pydantic 모델을 그대로 받으면 요청 바디 파싱, 유효성 검사, Swagger 문서 생성을 전부 자동으로 해준다. 타입 힌트 하나로 세 가지를 얻는 셈.
