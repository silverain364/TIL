# Spring Boot가 뭔지 5분 만에 이해하기
Java/Kotlin으로 백엔드 서버를 가장 빠르게 만드는 방법 | 2026-08-25

### 개념
Spring은 Java/Kotlin으로 백엔드를 만드는 가장 널리 쓰이는 프레임워크다. Spring Boot는 Spring을 더 쉽게 쓸 수 있게 만든 것으로, 복잡한 설정 없이 바로 서버를 띄울 수 있다. 국내 백엔드 채용의 상당수가 Spring을 요구할 만큼 실무에서 표준에 가깝다.

### SPIRNG vs SPRING BOOT
Spring은 강력하지마 XML 설정이 복잡하고 시작하기 어려움.</br>
Spring Boot는 설정 자동화 + 내장 톰캣 서버 포함 → 파일 하나로 바로 실행 가능. 지금은 Spring Boot가 사실상 표준이나 다름없다.

### Spring Boot 프로젝트 구조
`Controller` : 요청을 받고 응답을 돌려주는 계층. URL과 함수를 연결.
`Service` : 실제 비즈니스 로직이 있는 계층. 계산, 조건 처리 등.
`Repository` : DB와 소통하는 계층. 데이터 저장·조회·삭제.
`Entity` : DB 테이블과 1:1 대응하는 클래스.

> **start.spring.io에서 시작** : Spirng 프로젝트는 처음 만들 땐 start.spring.io에 접속해서 언어, 의존성을 고르고 zip을 다운받으면 된다. 직접 설정할 필요가 없다.</br>

> **앞으로 이어질 Spring TIL 흐름** : Controller → Service → Repository → JPA(DB 연결) → 예외처리 → 인증 순서로 차근차근 쌓기.
