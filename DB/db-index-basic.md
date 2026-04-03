# DB 인덱스 왜 쓰고, 언제 조심해야 하나?
인덱스는 조회를 빠르게 하지만, 무조건 많다고 좋은 게 아니다. | 2026-04-03

### 개념
인덱스는 책 뒤의 색인과 같다. 색인이 없으면 책 전체를 뒤져야(Full Table Scan) 하지만, 색인이 있으면 페이지 번호로 바로 찾아간다.
DB도 마찬가지로, 인덱스가 없는 컬럼을 WHERE로 걸면 전체 행을 다 읽는다.

인덱스 만들기
```
-- 단일 컬럼 인덱스
CREATE INDEX idx_users_email ON users(email);
-- 복합 인덱스 (순서 중요!)
CREATE INDEX idx_orders ON orders(user_id, created_at);
```

인덱스가 안 타는 이유
```
WHERE YEAR(created_at) = 2024 -- 함수로 감싸면 인덱스 못 쓴다.
WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31' -- 인덱스 사용됨.
```

```
WHERE name LIKE '%김' -- 앞에 % 붙으면 Full Scan
WHERE name LIKE '김%' -- 앞에서 시작해야 탐
```

**복합 인덱스는 왼쪽부터** -- INDEX(user_id, created_at)이면 user_id 단독 조회는 탈 수 있지만, created_at 단독 조회는 못 탄다.
**인덱스 남발 금지** -- 인덱스 많을수록 INSERT / UPDATE / DELETE가 느려진다. 쓰기가 많은 테이블엔 꼭 필요한 것만!
