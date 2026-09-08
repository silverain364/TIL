# loC와 DI
객체를 내가 만들지 않고, Spring이 만들어서 넣어준다.

### 개념
loC는 객체의 생성과 관리를 개발자가 아닌 Spring이 담당하는 것이다.</br>
DI는 그 방법으로, 필요한 객체를 Spring이 자동으로 넣어준다.</br>
Bean은 Spring이 관리하는 객체이다. @Service, @Repository 등으로 등록한다.

DI 없이 VS DI 있을 때
```java
class UserController {
  val service = UserService()

  fun getUser(id: Long) {
    service.findUser(id)
  }
}
// UserService가 바뀌면 해당 코드도 직접 수정
```

```java
@RestController
class UserController {
  // Spring이 알아서 넣어줌
  private val service: UserService
) {
    fun getUser(id: Long) {
    service.findUser(id)
  }
}
```

주요 BEAN 어노테이션
@Controller → 웹 요청 처리 계층</br>
@Service → 비즈니스 로직 계층</br>
@Repository → DB 접근 계층</br>
@Component → 위 셋에 해당 안 될 떄 범용으로 사용

> **Kotlin에선 생성자 주입이 깔끔하다.** Java는 @AutoWired를 필드에 붙이는 방식을 많이 쓰지만, Kotlin은 생성자 파라미터로 바로 받는 게 더 간결하고 권장되는 방식이다.<br/>

> DI가 좋은 이유는 테스트할 때 실제 UserService 대신 **가짜(Mock) 객체를 넣을 수 있기 때문이다.**
