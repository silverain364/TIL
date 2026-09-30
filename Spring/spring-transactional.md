# @Transactional 하나로 트랙잭션 관리하기!
2026-09-30

### Spring에서 트랜잭션이란??
트랜잭션은 "모두 성공하거나 모두 실패하거나"를 보장한다. 
Spring에서는 이걸 직접 Begin/commit/rollback을 직접 쓰지 않고, @Transacitionl 어노테이션 하나만 붙이면 Spring이 자동으로 처리해준다. 
메서드가 정상적으로 끝나면 커밋, 예외가 터지면 롤백이 자동으로 일어난다.

### 그럼 어느 부분에 붙이는 건가?
보통 Service 계층의 메서드에 붙인다고 한다. 비즈니스 로직이 Service에 있고, 트랜잭션도 비즈니스 로직 단위로 묶는 게 자연스럽기 때문이다.
Controller나 Repository에 붙이는 건 권장하지 않는다고 한다.

### 핵심 속성들
`readOnly= true` : 조회 전용 트랜잭션임을 명시한다. 쓰기 작업을 막고 성능 최적화가 된다. 데이터를 읽기만 하는 메서드엔 항상 붙이는 게 좋다.<br/>
`rollbackFor` : 기본적으로 RuntimeException만 롤백한다. Checked Exception도 롤백하고 싶다면 rollbackFor = [Exception::class]와 같이 명시해야 한다.<br/>
`propagation` : 트랜잭션이 이미 있을 떄 새 트랜잭션을 어떻게 처리할지 결정. 기본값은 REQUIRED로, 있으면 참여하고 없으면 새로 만든다. 처음엔 기본값으로 충분하다.

### 실제 사용 패턴
```java
// 클래스 전체에 readOnly = true 기본 설정
@Service
@Transactional(readOnly = true)
class OrderService (
  private val orderRepository: OrderRepository
) {
    // 조회 메서드 - readOnly 상속은 그대로
    fun getOrder(id: Long): Order {
      return orderRepository.findById(id)
        .orElseThrow { OrderNotFoundException(id) }
  }

    // 쓰기 메서드 - readOnly = false로 오버라이드
    @Transactional
    fun createOrder(request: OrderRequest): Order {
    // 재고 차감 + 주문 생성 했을 때 하나라도 실패하면 둘 다 롤백한다.
    val product = productRepository.findById(request.productId)
    product.decreaseStock(request.quantity)
    return orderRepository.save(Order(request))
  }
}
```

> 클래스에 readOnly = true, 쓰기 메서드에만 @Transactional을 사용한다. 이 패턴이 실무에서 가장 많이 쓰이는 방식이다. 클래스 전체를 읽기 전용으로 기본 설정하고, 데이터를 변경하는 메서드에만 @Transactional을 따로 붙여서 오버라이드한다.</br>

> 같은 클래스 내부 호출은 트랜잭션이 안 걸린다. @Transactional은 Spring이 프록시로 감싸서 동작하는데 같은 클래스 안에서도 메서드끼리 호출하면 프록시를 거치지 않아 트랜잭션이 무시된다.
