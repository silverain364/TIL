# Spring 예외 처리
예외가 터졌을 때 500 에러 HTML이 그대로 나가면 안 된다. | 2026-09-11

### 개념
아무 처리 안 하면 Spring은 예외가 발생했을 때 HTML로 된 에러 페이지를 내려준다. 
API 서버라면 JSON 형태의 일관된 에러 응답을 줘야 한다. 
Spring은 @ControllerAdvice와 @ExceptionHandler로 예외를 한 곳에서 모아서 처리할 수 있다.

### 커스텀 예외 클래스
RuntimeException 상속해서 상황별로 만들기</br>
@RestControllerAdvice → @ExceptionHandler로 전역 예외 처리</br>

> **@RestControllerAdvice = @ControllerAdvice + @ResponseBody** : JSON API 서버라면 @RestControllerAdvice를 쓰면 된다. 응답을 자동으로 JSON으로 변환해준다.</br>

> **예외마다 적절한 HTTP 상태 코드를 골라서 사용해야 된다.** 유저 없음 → 404, 중복 → 409, 서버 오류 → 500
