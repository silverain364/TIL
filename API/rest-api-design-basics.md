# REST API 설계 - URL을 잘 짜는 법
HTTP 메서드와 URL 구조만 제대로 써도 API가 읽기 쉬워진다. | 2026-04-13

### 개념
REST는 URL로 자원(Resource)을 표현하고, HTTP 메서드로 행위를 표현하는 설계 방식이다.<br/>
URL에 동사를 넣지 않고, 명사만 쓰는 게 핵심! 잘 설계된 API는 문서 없이도 어느 정도 읽힌다.

### HTTP 메소드 - 행위를 표현
`GET` : 조회. 서버 상태 변경 없음. `GET /user1/1`<br/>
`POST` : 생성. 새 리소스 만들기. `POST /users`<br/>
`PUT` : 전체 수정. 리소스 전체를 교체. `PUT /users/1`<br/>
`PATCH` : 부분 수정. 일부 피드만 변경. `PATCH /users/1`<br/>
`DELETE` : 삭제. `DELETE /users/1`

### URL 설계 - 좋은 예와 나쁜 예
동사 URL (나쁨)
```
GET /getUser/1
POST /createUser
POST /deleteUser/1
POST /updateUser/1
```

명사 URL (좋음)
```
GET /users/1
POST /users
DELETE /users/1
PATCH /users/1
```

### 중첩 리소스 - 관계 표현
```
GET /users/1/orders # 유저 1번의 주문 목록 
GET /users/1orders/3 # 유저 1번의 주문 중 5번 주문
GET /orders?status=pending&sort=created_at # 쿼리 파라미터로 필터링/정렬
```

> **URL은 명사 복수형으로** - /user보다 /users가 관례다. 행위에서는 메서드로 표현하고, URL엔 동사를 쓰지 않는다. 단, 로그인처럼 리소스가 아닌 행위는 POST /auth/login 같은 예외를 허용하기도 한다.
