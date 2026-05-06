# Redis - DB 앞에 세워두는 초고속 메모리 저장소
같은 쿼리를 매번 DB에 날리는 건 낭비다. Redis로 캐싱하면 수십 배 빨라진다. | 2026-05-06

### 개념
Redis는 데이터를 디스크가 아닌 메모리(RAM)에 저장하는 Key-Value 저장소다. 읽기 속도가 DB보다 수십~수백 배 빠르다.<br/>
캐시, 세션 저장, 실시간 순위 등 "빠르게 읽어야 하는 데이터"에 쓴다. 단, 서버가 꺼지면 데이터가 날라갈 수 있다.

자주 쓰는 명령어
```
# 문자열 저장 및 조회<br/>
SET user:1:name "김철수" GET user:1:name<br/>
# 만료 시간 설정 (초 단위)- 세션, 캐시에 필수<br/>
SET token:abc123 "user_id:1" EX 3600 TTl token:abc123<br/>
# 삭제<br/>
DEL user:1:name
```

PYTHON에서 사용하기
```
# pip install redis<br/>
import redis r = redis.Redis(host='localhost', port=6379, db=0)<br/>
# 캐싱 패턴 - DB 조회 전 Redis 먼저 확인<br/>
def get_user(user_id): cache_key = f"user:{user_id}<br/>
# 캐시 히트 - DB 안 감<br/>
user = db.query(f"SELECT * FROM users WHERE id={user_id}") r.set(cache_key, user, ex=300)<br/>
# 5분 캐시<br/>
return user
```

<hr>

redis가 빛나는 상황
```
세션/토큰 저장 - 로그인 토큰을 EX로 만료 시간 설정해서 저장.
```
```
> API 응답 캐싱 - 자주 조회되는 데이터를 캐시해 DB 부하 감소.
```
```
> 실시간 랭킹 - Sorted Set으로 점수 기반 순위를 실시간 처리.<br/>
```
```
> Rate Limiting - 요청 횟수를 카운트해서 초당 요청 수 제한.
```

> **캐시 무효화가 핵심** 캐시된 데이터가 DB와 달라지는 순간이 문제다. 데이터가 수정될 때 관련 캐시 키를 지워주는 습관이 필요하다.<br/>
> **키 네이밍 관례** 타입:id:필드 형식으로 계층적으로 짓는다. 예:user:1:profile, order:42:status
