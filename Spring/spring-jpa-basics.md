# JPA - SQL 없이 DB를 다루는 방법
클래스와 DB 테이블을 연결해서, 객체를 저장하면 DB에 자동으로 들어간다. | 2026-09-10

### 개념
JPA(Java Persistence API)는 Java/Kotlin 객체와 DB 테이블을 자동으로 연결해주는 기술이다. 
SQL을 직접 쓰는 대신 객체를 저장·조회·삭제하면 JPA가 SQL을 대신 만들어준다. 
Spring에서는 JPA 구현체로 Hibernate를 사용하고, Spring Data JPA가 이를 더 편하게 감싸준다.

### Entity - db 테이블과 연결되는 클래스
```java
import jakarta.persistence.*

@Entity // 이 클래스가 db와 연결됨.
@Table(name = "users")
class User(
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  val id: Long = 0,

  @Column(nullable = false)
  val name: String,

  @Column(unique = true)
  val email: String
)
```

### Repository - db 조작은 여기서
```java
import org.springframework.data.jpa.repository.JpaRepository

interface UserRepository : JpaRepository<User, Long>

userRepository.save(user) // INSERT / UPDATE
userRepository.findById(1L) // SELECT WHERE id = 1
userRepsository.findAll() // SELECT *
userRepository.deleteById(1L) //DELETE WHERE id = 1
```

### 주요 어노테이션
`@Entity` - DB 테이블과 매핑되는 클래스 선언.<br/>
`@Id` - 기본키 필드 지정.<br/>
`@GenerateValue` - PK 자동 생성 전략.<br/>
`@Column` - 컬럼 세부 설정.

> **메서드 이름으로 쿼리가 만들어진다.** JpaRepository에 findByEmail(email: String)이라고 선언만 해도 Spring이 자동으로 SELECT * FROM users WHERE email = ? 쿼리를 만들어준다. 이걸 메서드 이름 쿼리라고 한다.</br>

> **application.yml 설정 필요하다.** JPA를 쓰려면 DB 연결 정보가 있어야 한다. spring.deatasource.url, spring.jpa.hibernate.ddl-auto 등을 설정해줘야 동작한다.
