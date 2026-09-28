# Kotlin Null 안전성
kotlin에서 null 처리가 중요한 이유는? | 2026-09-28

### Null이 왜 문제일까?
Java에서 흔한 오류는 NullPointException이다. 값이 있다고 믿고 접근했는데 null이라 터지는 것인데 컴파일할 때는 전혀 보이질 않다가 실제로 실행되어야 터지는 걸 보고 확인이 된다.
Kotlin은 이런 문제를 타입 시스템 자체에서 해결하였다. null이 들어올 수 있는 변수와 절대 null이 아닌 변수를 타입 단계에서 구분해서 잘못 접근하면 컴파일 자체가 안 된다.

### Null 처리 연산자 4가지
`?.` : 안전 호출 연산자. 앞의 값이 null이면 그냥 null을 반환한다. null이어도 NPE가 터지지 않는다. 체이닝도 가능해서 user?.address?.city처럼 중간에 하나라도 null이면 전체가 null이 된다.</br>
`?:` : 엘비스 연산자. 앞의 값이 null일 때 대신 쓸 기본값을 지정한다. nickname ?: "이름 없음"은 nickname이 null이면 "이름 없음"을 쓰라는 뜻이다. Java의 삼항 연산자 null 체크를 한 줄로 줄여준다.</br>
`!!` : non-null 단언 연산자. "이 값은 절대 null이 아니다."라고 개발자가 직접 보증하는 것이다. null이어도 컴파일은 되지만, 실제로 null이면 NPE가 터진다. 확실히 null이 아닌 게 보장될 때만 쓰고, 남잘하면 Kotlin의 null 안전성을 포기하는 것과 다름없다.<br/>
`let` : null이 아닐 때만 블록 실행하는 것을 뜻한다. nickname?.let { println(it) }처럼 쓰면 nickname이 null이 아닐 때만 블록 안 코드가 실행된다. if (nickname != null) 체크를 더 간결하게 표현할 수 있다.

### 실제 사용 예시
```java
// ?. - null이면 그냥 넘어감
val city = user?.address?.city

// ?: - null이면 기본값
val displayName = user?.nickname ?: "익명"

// ?. + ?: - 가장 자주 쓰는 조합. 삼항 연산자 활용.
val length = nickname?.length ?: 0

// let - null이 아닐 때만 실행
user?.let{
  println("로그인 유저: ${it.name}")
}

// !! - 진짜 확실할 때만
val name = user!!.name
```

> 코드에 !!가 많이 보이면 Kotlin을 java처럼 쓰고 있다는 신호다. 대부분의 경우 ?.와 ?: 조합으로 해결 가능하다.</br>

> JPA Entity의 필드를 var로 선언하고 기본값 없이 쓰면 null 관련 문제가 생기기 쉽다. Kotlin에서 JPA를 쓸 땐 초기화 방식을 신경 써야 한다.
