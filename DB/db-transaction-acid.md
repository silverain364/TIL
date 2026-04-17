# 트랜잭션과 ACID - 데이터가 망가지지 않는 이유
계좌 이체가 중간에 실패해도 돈이 사라지지 않는 건 트랜잭션 덕분이다. | 2026-04-17

### 트랜잭션은 "모두 성공하거나, 모두 실패하거나"를 보장하는 작업 단위다.<br/>
A 계좌에서 돈을 빼고 B 계좌에 넣는 두 쿼리 중 하나만 실행되면 큰일 나듯이 트랙잭션으로 묶으면 중간에 오류가 나도 전부 롤백한다.

**ACID 4가지 성질**
`A` Atomicity 원자성 : 전부 성공 또는 전부 실패. 중간 사앹 없음.<br/>
`C` Consistency 일관성 : 트랜잭션 전후로 DB 규칙이 항상 유지됨.<br/>
`I` Isolation 격리성 : 동시에 실행되는 트랜잭션이 서로 영향 안 줌.<br/>
`D` Duability 지속성 : 커밋된 데이터는 장애가 나도 사라지지 않음.<br/>
<br/>
**실제 SQL - 계좌 이체 예시**
```
트랜잭션 없이 중간 실패 시 돈 증발<br/>
UPDATE accounts SET balance = balance - 10000<br/>
WHERE id = 1; UPDATE accounts SET balance = balance + 10000 WHERE id = 2;
```
```
트랜잭션으로 묶기. 둘 다 서옹해야 반영<br/>
BEGIN;<br/>
UPDATE accounts SET balance = balance - 10000 WHERE id = 1;<br/>
UPDATE accounts SET balance + 10000 WHERE id= 2;<br/>
COMMIT;<br/>
오류 발생 시 ROLLBACK;
```

**PYTHON(SQLALCHEMY) 예시**
```
conn.execute("UPDATE accounts SET balace = balace - 10000 WHERE id = 1")<br/>
conn.execute("UPDATE accounts SET balace = balace - 10000 WHERE id = 2")<br/>
```
> **트랜잭션은 짧게 해야 된다.** 트랙잭션이 길어질수록 DB 락을 오래 잡는다.
