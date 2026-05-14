# CI/CD - 코드 push 한 번으로 배포까지 자동화하기
매번 손으로 서버에 올리는 건 옛날 방식이다. | 2026-05-14

### 개념
CI(Continuous Integration)는 코드를 push할 때마다 자동으로 테스트 및 빌드를 돌리는 것이고,<br/>
CD(Continuous Deployment)는 그 결과를 자동으로 서버에 배포하는 것이다.<br/>
Github Actions, GitLab CI, Jenkins 등이 대표적인 도구다. 한 번 세팅해두면 "git push"만으로 배포가 끝난다.

CI/CD 파이프라인 흐름
1. `git push` -> GitHub에 코드 올라감
2. `CI 실행` -> 테스트 자동 실행, 빌드 확인
3. `성공 시` -> Docker 이미지 빌드 & 레지스트리에 push
4. `CD 실행` -> 서버에 새 이미지 pull & 재시작

> **테스트가 없으면 CI가 의미없다.** CI의 핵심은 자동 테스트다. 테스트 코드 없이 빌드만 확인하는 건 절반짜리다. 간단한 테스트라도 먼저 작성하는 습관이 중요하다.<br/>
> **브랜치 전략과 연결** 보통 main push -> 운영 배포, dev push -> 스테이징 배포로 분리해서 운영한다.
