# 잘못된 요청을 입구에서 막기 (Spring Validation)
빈 이름이나 이상한 이메일 형식을 Service까지 넘기지 말고 Controller에서 바로 거르자 | 2026-09-14

### 그럼 왜 요청을 처음부터 막는 게 필요한가?
클라이언트에서 오는 요청은 믿을 수 없는 경우가 대부분이다. 이메일 형식이 맞지 않거나, 나이에 음수를 넣는다는 등의 요청이 들어오는 때가 많기에 이 데이터들을 직접 처리해주는 Service 및 DB로 오게 되면 더 복잡한 오류가 생긴다.
때문에 **요청이 들어오는 입구인 Controller에서 미리 검증을 통해서 필터링하는 것이 가장 깔끔하다.**
어노테이션 중 @Valid 하나만 붙이면 요청 객체의 필드에 달린 검증 규칙을 자동으로 확인해준다. 검증 실패 시 400 Bad Request를 반환한다.

### 자주 쓰는 어노테이션
`@NotBlank` : null도 안 되고, 빈 문자열도 안 되고(""), 공백만 있어도 안 됨.</br>
`@NotNull` : null만 안 됨. 빈 문자열 등은 허용. 숫자나 Boolean 타입에 주로 씀.</br>
`@Email` : 이메일 형식인지 자동으로 체크. @ 포함 여부 등의 기본적인 형식 검증.</br>
`@Size(min, max)` : 문자열 길이 범위를 제한. 비밀번호와 같은 규칙에 사용.</br>
`@Min / @Max` : 숫자의 최솟값/최댓값 제한. 나이가 0보다 작으면 안 될 때 등.</br>
`@Pattern` : 정규식으로 형식 검증. 전화번호 형식, 특수문자 제한 등 복잡한 규칙에 사용.</br>

### Valid 동작 흐름
1. 요청 객체 (data class)의 필드에 검증 어노테이션을 달아놓고, Controller 파라미터에 @Valid를 붙인다.
2. Spring이 요청을 받는 순간 request값을 통해 요청 객체가 처리하게 된다.
3. request값을 검증한 뒤 실패하면 MethodArgumentNotValidException을 던진다.

### 예시
```java
// 요청 객체에 검증 규칙 선언
data class CreateUserRequest(
  @field:NotBlank(message = "이름은 필수입니다.")
  val name: String,

  @field:Email(message = "이메일 형식이 올바르지 않습니다.")
  val email: String,

  @field:Min(value = 0, message = "나이는 0 이상이어야 합니다.)
  val age: Int

// Controller에서 @Valid 추가
@PostMapping
fun createUser(@Valid @RequestBody(Body request: CreateUserRequest): User {
  return userService.createUser(request)
}
```

> **Kotlin에서는 @field:를 붙여야 한다.** Kotlin의 data class는 생성자 파라미터에 어노테이션을 달 때 @field: 접두사가 필요하다. 안 붙이면 검증이 동작하지 않는 경우가 생긴다.</br>

> Validation 실패 시 던지는 MethodArgumentNotValidException을 GlobalExceptionHandler에서 잡아서 어떤 필드가 왜 실패했는지 메세지로 담아 응답하면 완성된다.
