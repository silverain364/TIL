# Rate Limiting - API를 무한정 두드리지 못하게 막기
초당 1000번 요청을 허용하면 서버는 금방 뻗는다.

### 개념
Rate Limiting은 특정 클라이언트가 일정 시간 안에 보낼 수 있는 요청 횟수를 제한하는 기법이다. 악의적인 공격(DDoS, 브루트포스)를 막고, 서버 과부하를 방지한다. 한도를 초과하면 429 Too Many Requests를 응답한다.

### 주요 알고리즘 3가지
`Fixed Window` - 1분 단위로 카운터 리셋. 구현 쉬움. 단, 창 경계 직전 및 직후에 2배 요청 가능한 허점 있음.</br>
`Sliding Window` - 현재 시각 기준 최근 N초를 계산. Fixed Window의 허점 없음. Redis로 구현.</br>
`Token Bucket` - 버킷에 토큰이 계속 채워지고, 요청마다 토큰 소모. 순간 버스트 허용. AWS API Gateway 기본 방식.</br>

> **IP 기반 vs 유저 기반** 로그인 전에는 IP로, 로그인 후에는 user_id로 제한하는 게 일반적이다. IP만 쓰면 같은 회사 NAT 뒤에 있는 수백 명이 같이 막힐 수 있다.</br>

> **Redis + HTTP 429와 연결** Rate Limiting은 Redis의 Sorted Set으로 구현하고, 초과 시 429를 반환한다. 여기까지 오면 앞에서 배운 개념들이 실제로 맞물리는 게 보이기 시작한다.
