# Kotlin이 Java와 다른 점은?
Java를 알면 Kotlin은 "더 짧고 안전하게 쓰는 Java"로 이해하면 된다. | 2026-09-18

### Kotlin을 선택하는 이유는?
Kotlin은 JetBrains이 만든 언어로 JVM 위에서 돌아가서 Java와 100% 호환된다. 기존 Java 라이브러리를 그대로 쓸 수 있고, Spring도 Kotlin을 공식 지원한다. 
Java보다 코드가 훨씬 짧고, NullPointerException을 컴파일 타임에 방지하는 구조 덕분에 안정성도 높아서 최근 백엔드에서 빠르게 퍼지고 있다.

### Java vs Kotlin 비교하기
```java
public class User {
  private String name;
  private int age;

  public User(
    String name,
    int age
  ) {
    this.name = name;
    this.age = age;
  }

  public String getName() {
    return name;
  }
  //getter, setter 등등
}
```

```java
data class User(
  val name: String,
  val age: Int
)
// 4줄로 동일한 기능을 한다.
```

### 꼭 알아야 할 Kotlin 특징 3가지
1. `val` vs `var` : `val`은 한 번 할당하면 바꿀 수 없는 변수(Java의 final), var는 바꿀 수 있는 변수다. Kotlin에서는 기본적으로 val을 쓰고, 꼭 변경이 필요할 때만 var를 쓰는 게 관례다. 코드가 예측 가능해지고 버그가 줄어든다.</br>
2. Null의 안정성 : Kotlin은 기본적으로 모든 변수가 null을 허용하지 않는다. null이 들어올 수 있는 변수는 타입 뒤에 ?을 붙여서 명시해줘야 한다. 그래서 Java에서 자주 터지던 NullPointerException을 컴파일 단계에서 미리 잡아낼 수 있다.<br/>
3. data class : data class를 쓰면 equals, hashCode, toString, copy 메서드를 자동으로 만들어준다. Java에서 Lombok 같은 라이브러리로 해결하던 보일러프레잉트 코드가 필요 없어진다. Spring에서 DTO나 요청 객체를 만들 때 거의 항상 data class를 쓰는 이유이다.</br>

### Null 안정성 코드 예시
```java
// null 불가
val name: String = "철수"

// null 허용 - ? 붙여야 함
val nickname: String? = null

// null일 수도 있느 값 안전하게 접근
val length = nickname?.length

// null이면 기본값 사용
val display = nickname ?: "이름 없음"
```

> **?.는 "null이면 그냥 넘어가", ?:는 "null이면 이걸 써"라고 기억하며 쉽다.** 이 두 가지만 익숙해져도 Kotlin의 null 처리가 훨씬 편해진다.</br>

> **Java 코드와 섞어서 쓸 수 있다.** Kotlin과 Java는 같은 프로젝트에 공존할 수 있어서, 기존 Java 프로젝트에 Kotlin을 조금씩 도입하는 것도 가능하다.
